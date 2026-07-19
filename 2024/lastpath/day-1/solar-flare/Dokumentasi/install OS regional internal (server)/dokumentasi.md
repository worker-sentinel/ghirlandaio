# Dokumentasi Instalasi OS — Regional Internal (Server)

**Role regional:** Internal — menjalankan engine cache, hanya boleh diakses oleh Regional Control.
**Tipe mesin:** Server (mengikuti rule server CIS — minim paket, layanan dibatasi sesuai fungsi).
**Skema disk:** Full Disk Encryption — LUKS dibuat dulu di atas partisi mentah, baru LVM dibangun di atas volume terenkripsi tersebut (LUKS-on-partition → LVM-on-LUKS).

---

## 1. Melihat partisi disk

```
lsblk
```
**Penjelasan:** Menampilkan daftar block device dan partisi yang tersedia di sistem. Dilakukan di awal untuk memastikan target partisi yang akan dipakai sudah benar sebelum melakukan operasi destruktif (format, hapus VG, dll), supaya tidak salah menghapus disk lain. Pada server ini, partisi target adalah `nvme0n1p6`.

---

## 2. Membersihkan sisa LVM lama (jika ada)

> Jika `lsblk` menunjukkan masih ada Volume Group/Logical Volume lama di atas partisi target (sisa percobaan instalasi sebelumnya), partisi harus dibersihkan total dulu sebelum dipakai ulang — baik untuk skema terenkripsi maupun tidak.

```
umount -R /mnt
vgremove -f (nama_vg_lama)
pvremove /dev/nvme0n1p6
lsblk
```
**Penjelasan opsi:**
- `umount -R /mnt` — melepas seluruh mount point yang masih aktif di `/mnt` secara rekursif (`-R`), supaya partisi di bawahnya tidak "terkunci" saat mau dihapus.
- `vgremove -f` — menghapus Volume Group beserta seluruh Logical Volume di dalamnya sekaligus. Opsi `-f` (force) supaya tidak perlu konfirmasi satu per satu untuk tiap LV.
- `pvremove` — menghapus label LVM dari partisi, mengembalikannya menjadi partisi mentah/polos.
- `lsblk` — verifikasi ulang bahwa partisi sudah bersih (tidak ada lagi baris bertipe `lvm` di bawahnya) sebelum lanjut ke langkah berikutnya.

---

## 3. Membuat dan membuka volume LUKS (enkripsi)

> Ini **harus dilakukan sebelum** membuat PV/VG/LV apa pun. Kalau LVM dibangun dulu baru mencoba `cryptsetup luksFormat` di partisi yang sama, akan muncul error `Cannot exclusively open ..., device in use` karena partisi sudah "ditempel" sebagai PV/VG yang aktif.

```
cryptsetup luksFormat /dev/nvme0n1p6
cryptsetup open /dev/nvme0n1p6 cryptinternal
```
**Penjelasan opsi:**
- `cryptsetup luksFormat /dev/nvme0n1p6` — menginisialisasi partisi mentah sebagai container LUKS terenkripsi. Sistem akan meminta konfirmasi `YES` (huruf besar, karena ini operasi destruktif/irreversible) dan passphrase enkripsi.
- `cryptsetup open /dev/nvme0n1p6 cryptinternal` — membuka container LUKS yang baru dibuat dan memetakannya sebagai device mapper bernama `cryptinternal` di `/dev/mapper/cryptinternal`. Nama mapper ini bebas, tapi **harus konsisten** dipakai lagi nanti di konfigurasi `/etc/cmdline.d` (langkah 17) supaya initramfs tahu device mana yang harus dibuka saat boot.

---

## 4. Membuat Physical Volume di atas device terenkripsi

```
pvcreate /dev/mapper/cryptinternal
```
**Penjelasan:** Menginisialisasi device mapper hasil buka LUKS (bukan partisi mentahnya lagi) sebagai Physical Volume LVM. Karena PV dibuat di atas `cryptinternal`, semua Logical Volume yang dibuat setelahnya otomatis ikut terenkripsi.

---

## 5. Membuat Volume Group

```
vgcreate internal /dev/mapper/cryptinternal
```
**Penjelasan:** Mengelompokkan Physical Volume ke dalam satu Volume Group bernama `internal`, sesuai nama regional agar mudah diidentifikasi saat administrasi maupun audit (bukan `admin`, yang merupakan nama regional Control — jangan sampai tertukar antar server).

---

## 6. Membuat Logical Volume

```
lvcreate -L 10G internal -n root
lvcreate -L 10G internal -n vars
lvcreate -L 10G internal -n vlog
lvcreate -L 10G internal -n vtmp
lvcreate -L 10G internal -n vaud
lvcreate -L 10G internal -n home
lvcreate -L 10G internal -n podman
```
**Penjelasan opsi:**
- `-L 10G` — menentukan ukuran tetap (fixed size) tiap Logical Volume. Ukuran tetap (bukan persentase) dipilih agar partisi sensitif seperti `vlog` dan `vaud` (audit log) punya kapasitas terjamin dan tidak bisa "dimakan" oleh partisi lain.
- `-n <nama>` — nama Logical Volume, dipisah per fungsi (root, var, log, tmp, audit, home, container storage) sesuai prinsip **separation of partitions** di CIS Benchmark — tujuannya agar satu direktori yang penuh (misal log membludak) tidak mematikan seluruh sistem, dan agar tiap partisi bisa diberi opsi mount keamanan berbeda (`noexec`, `nosuid`, dll) sesuai fungsinya.

---

## 7. Memformat filesystem

```
mkfs.vfat -F32 -n BOOT /dev/(partisi_boot)
mkfs.ext4 /dev/internal/root
mkfs.ext4 /dev/internal/vars
mkfs.ext4 /dev/internal/vlog
mkfs.ext4 /dev/internal/vtmp
mkfs.ext4 /dev/internal/vaud
mkfs.ext4 /dev/internal/home
mkfs.ext4 /dev/internal/podman
```
**Penjelasan opsi:**
- `mkfs.vfat -F32 -n BOOT` — partisi boot/EFI wajib FAT32 (`-F32`) karena merupakan standar UEFI; `-n BOOT` memberi label agar mudah dikenali. Partisi boot **tidak** ikut dienkripsi karena UEFI firmware perlu membacanya sebelum LUKS bisa dibuka.
- `mkfs.ext4` — filesystem ext4 dipilih untuk partisi Linux karena stabil, mendukung journaling (memulihkan data jika sistem mati mendadak), dan umum dipakai di server produksi.

---

## 8. Mounting

```
mount /dev/internal/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi_boot) /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/internal/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/internal/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/internal/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/podman /mnt/var/lib/containers
```
**Penjelasan opsi mount (sesuai CIS hardening):**
- `uid=0,gid=0` — partisi boot di-mount sebagai milik root sehingga user biasa tidak bisa memodifikasi file boot.
- `fmask=0077,dmask=0077` — membatasi permission file dan direktori di partisi boot hanya bisa diakses root.
- `nodev` — mencegah pembuatan device file di partisi tersebut (mencegah penyalahgunaan device khusus untuk eskalasi privilege).
- `nosuid` — mencegah bit SUID/SGID berfungsi di partisi tersebut, mencegah binary di sana dipakai untuk eskalasi privilege.
- `noexec` — mencegah file di partisi tersebut dieksekusi langsung; diterapkan ke `/var/tmp`, `/var/log`, `/var/log/audit`, dan `/var/lib/containers` karena partisi ini seharusnya hanya menyimpan data, bukan menjalankan program.
- `relatime` — mengurangi penulisan metadata access-time berlebihan demi performa, tanpa mematikan atime sepenuhnya.
- `/var/log/audit` dipisah dari `/var/log` agar audit log (auditd) tidak ikut terhapus/penuh bersamaan dengan log umum — penting untuk keperluan forensik.
- `/var/lib/containers` dipisah karena server Internal menjalankan Podman (engine cache dijalankan sebagai container) — partisi terpisah mencegah data container memenuhi partisi root.

---

## 9. Install sistem operasi dasar(pacstrap)

```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld podman podman-compose openssh
```
**Penjelasan paket:**
- `intel-ucode` — microcode update untuk CPU Intel (ganti `amd-ucode` jika server pakai CPU AMD).
- `linux-lts`, `linux-lts-headers` — kernel LTS (Long Term Support) untuk stabilitas server jangka panjang.
- `lvm2`, `mkinitcpio` — wajib karena sistem memakai LVM dan butuh initramfs custom untuk boot dari volume terenkripsi.
- `firewalld` — manajemen firewall, wajib di-*enable* nanti untuk membatasi akses (lihat langkah 16).
- `podman`, `podman-compose` — engine container untuk menjalankan engine cache di regional Internal.
- `openssh` — agar server Internal bisa diakses jarak jauh oleh Regional Control sesuai rules akses.
- Paket GUI seperti `firefox` sengaja **tidak** dipasang karena server bersifat headless (prinsip *minimal install / reduce attack surface*). `asciinema` juga tidak perlu terpasang di server target — itu dipakai di sisi workstation kalian sendiri untuk merekam proses instalasi.

---

## 10. Generate fstab

```
genfstab -U /mnt > /mnt/etc/fstab
```
**Penjelasan:** Membuat file `/etc/fstab` otomatis berdasarkan mount point yang sedang aktif. Opsi `-U` memakai UUID (bukan path device `/dev/sdX`) supaya konfigurasi tetap valid meski urutan device berubah saat boot berikutnya.

---

## 11. Menambahkan tmpfs untuk /tmp

```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
**Penjelasan:** `/tmp` dipasang sebagai tmpfs (di RAM) dengan opsi `nosuid,noexec` agar file temporer tidak bisa dieksekusi atau dipakai untuk eskalasi privilege, dan `size=1G` membatasi penggunaan RAM agar tidak menghabiskan memori sistem.

---

## 12. Mengecek hasil konfigurasi fstab

```
cat /mnt/etc/fstab
```
**Penjelasan:** Verifikasi manual bahwa seluruh entri mount (termasuk opsi keamanan tiap partisi) sudah tercatat benar sebelum melanjutkan, agar tidak perlu mengulang dari awal jika ada kesalahan.

---

## 13. Masuk ke environment sistem baru

```
arch-chroot /mnt
```
**Penjelasan:** Berpindah root environment ke sistem yang baru diinstall di `/mnt`, sehingga perintah-perintah selanjutnya (locale, user, bootloader) dijalankan seolah-olah sudah boot ke sistem baru tersebut.

---

## 14. Konfigurasi zona waktu

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
**Penjelasan:** `ln -sf` membuat symlink zona waktu sistem ke Jakarta (WIB), `-f` memaksa overwrite jika symlink lama sudah ada. `hwclock --systohc` menyinkronkan jam hardware (RTC) dengan jam sistem agar konsisten setelah reboot.

---

## 15. Konfigurasi bahasa sistem (locale)

```
nvim /etc/locale.gen
```
> Cari baris `en_US.UTF-8 UTF-8`, hapus tanda pagar (`#`) di depannya agar locale tersebut aktif digenerate.

```
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
echo "LC_ALL=en_US.UTF-8" >> /etc/locale.conf
```
**Penjelasan:** `locale-gen` membaca `/etc/locale.gen` dan menggenerate locale yang sudah diaktifkan. `/etc/locale.conf` lalu diisi `LANG` dan `LC_ALL` agar sistem konsisten memakai encoding UTF-8 untuk semua kategori locale (mencegah masalah karakter aneh saat menjalankan command/log).

---

## 16. Membuat user dan akses sudo

```
useradd -m (nama_user)
passwd (nama_user)
echo "(nama_user) ALL=(ALL:ALL) ALL" > /etc/sudoers.d/(nama_user)
chmod 0440 /etc/sudoers.d/(nama_user)
```
**Penjelasan opsi:**
- `useradd -m` — `-m` membuat home directory otomatis untuk user baru.
- `passwd` — mengatur password login user tersebut.
- File di `/etc/sudoers.d/` dibuat terpisah per user (bukan langsung edit `/etc/sudoers`) agar lebih aman diaudit dan tidak berisiko merusak file sudoers utama.
- `chmod 0440` — sudoers menolak file dengan permission yang terlalu longgar, jadi permission wajib diset ke `0440` (read-only untuk owner & group, tidak ada akses untuk other).

---

## 17. Konfigurasi kernel cmdline (untuk boot dari LUKS)

```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6)=cryptinternal" > /etc/cmdline.d/01-boot.conf
echo "rd.lvm.lv=internal/root root=/dev/internal/root" >> /etc/cmdline.d/01-boot.conf
echo "rw" > /etc/cmdline.d/02-misc.conf
```
**Penjelasan opsi:**
- `rd.luks.name=<UUID>=cryptinternal` — memberitahu initramfs UUID partisi LUKS mana yang harus dibuka, dan nama mapper yang dipakai (`cryptinternal`, konsisten dengan langkah 3) saat boot.
- `rd.lvm.lv=internal/root` — memberitahu initramfs Logical Volume mana dari VG `internal` yang harus diaktifkan sebagai root filesystem. Nama VG di sini harus sama persis dengan yang dipakai di langkah 5 (`internal`).
- `rw` — boot dengan root filesystem mode read-write (bukan read-only), karena server butuh menulis ke root saat berjalan normal.

```
nvim /etc/mkinitcpio.conf
```
> Ubah baris `HOOKS` menjadi:
```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block sd-encrypt lvm2 filesystems fsck)
```
**Penjelasan:** Hook `sd-encrypt` dipakai karena memakai skema initramfs berbasis systemd (sesuai hook `kms`, `keyboard`, `consolefont` yang juga berbasis systemd-hooks). Hook ini wajib diletakkan **sebelum** `lvm2` karena urutan pemrosesan saat boot: device → buka enkripsi (`sd-encrypt`) → baru aktifkan LVM di atasnya (`lvm2`) → mount filesystem (`filesystems`) → cek filesystem (`fsck`).

---

## 18. Konfigurasi preset initramfs

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
> Sunting bagian berikut (hilangkan tanda pagar `#` pada baris yang relevan, dan sesuaikan path):
```
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"

PRESETS=('default')

default_image="/boot/initramfs-linux-lts.img"
```
**Penjelasan:** Path file kernel dan initramfs harus konsisten huruf kecil sesuai nama paket (`linux-lts`), dan ekstensi konfigurasi memakai `.conf`.

---

## 19. Instalasi bootloader

```
bootctl --path=/boot install
```
**Penjelasan:** Memasang `systemd-boot` sebagai bootloader UEFI ke partisi `/boot` (EFI System Partition). `systemd-boot` dipilih karena ringan dan terintegrasi baik dengan ekosistem systemd yang juga dipakai untuk hook initramfs (`sd-encrypt`).

---

## 20. Membuat initramfs

```
mkinitcpio -P
```
**Penjelasan:** Opsi `-P` (preset) menggenerate ulang initramfs untuk semua preset kernel yang terdaftar di `/etc/mkinitcpio.d/`, menerapkan perubahan HOOKS dan preset yang sudah dikonfigurasi sebelumnya.

---

## 21. Mengaktifkan layanan jaringan dan keamanan

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
```
**Penjelasan:**
- `systemd-networkd` — manajemen konfigurasi jaringan (IP, routing).
- `systemd-resolved` — manajemen DNS resolver.
- `iwd` — daemon WiFi (relevan bila server pakai koneksi wireless; bila server selalu pakai kabel/ethernet, opsional).
- `firewalld` — wajib di-enable agar firewall otomatis berjalan saat boot (paketnya sudah dipasang di langkah 9).
- `sshd` — agar Regional Control bisa mengakses server Internal dari jarak jauh sesuai rules akses pada arsitektur tugas.

> **Langkah lanjutan (dilakukan setelah reboot):** konfigurasi rule firewall (`firewall-cmd`) agar port SSH/akses cache **hanya** menerima koneksi dari IP/subnet Regional Control, sesuai prinsip *least privilege* pada arsitektur jaringan tugas ini.

---

## 22. Keluar dari chroot dan reboot

```
exit
umount -R /mnt
reboot
```
**Penjelasan:** `exit` keluar dari environment chroot. `umount -R /mnt` melepas seluruh mount point secara rekursif sebelum reboot, untuk memastikan tidak ada filesystem yang masih "terkunci" sehingga proses unmount-saat-boot tidak korup.

---

## Catatan Tambahan

- Saat boot pertama kali setelah instalasi, sistem akan meminta passphrase LUKS untuk membuka `cryptinternal` sebelum bisa lanjut ke proses boot — ini normal dan menandakan enkripsi berjalan.
- Jika tidak ingin memakai enkripsi sama sekali (misalnya karena keterbatasan waktu pengerjaan), langkah 3 (LUKS) dan referensi `cryptinternal` di langkah 17 bisa dilewati — langkah 4 (`pvcreate`) langsung dijalankan di atas partisi mentah (`/dev/nvme0n1p6`), dan hook `sd-encrypt` di langkah 17 dihapus dari `HOOKS`.
- Pastikan nama VG (`internal`) dan nama mapper (`cryptinternal`) konsisten dipakai di seluruh langkah — kesalahan penamaan adalah penyebab paling umum sistem gagal boot.
