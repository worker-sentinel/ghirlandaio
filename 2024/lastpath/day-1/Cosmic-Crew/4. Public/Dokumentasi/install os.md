# Dokumentasi Instalasi OS format CIS

Dokumentasi ini berisi panduan langkah demi langkah instalasi advanced Arch Linux menggunakan enkripsi **LUKS**, manajemen volume **LVM**, pemisahan direktori untuk keamanan server (*hardening*), serta pembuatan **Unified Kernel Image (UKI)** berbasis **Systemd-boot**.

---

## 1. Connect Wi-Fi

Bagian ini digunakan untuk menyambungkan perangkat ke jaringan internet via nirkabel menggunakan utility bawaan installer (`iwd`).

```
iwctl
```
Masuk ke dalam *interactive prompt* atau konsol kontrol milik internet Wireless Daemon (iwd).
```
device list
```
Menampilkan daftar kartu/antarmuka (interface) Wi-Fi yang tersedia pada perangkat (contoh: `wlan0`).
```
station wlan0 connect (nama wifi)
```
Memerintahkan kartu Wi-Fi (`wlan0`) untuk menyambungkan ke nama SSID Wi-Fi tujuan. Sistem akan meminta password setelah perintah ini dieksekusi.
 ```
exit
```
Keluar dari prompt interaktif `iwctl` dan kembali ke terminal utama.
```
ping 1.1.1.1
```
Mengirim paket uji coba ke DNS Cloudflare untuk memastikan koneksi internet sudah aktif dan stabil.

---

## 2. Membuat Partisi

Mengatur skema ruang penyimpanan pada harddisk atau SSD sebelum diisi oleh sistem operasi.

* **`cfdisk /dev/(partisi)`** Membuka teks antarmuka (TUI) interaktif untuk membagi ruang *drive* induk (misal: `/dev/sda` atau `/dev/nvme0n1`).

> **Skema Pembagian Partisi yang Digunakan:**
> * `boot` = **4G** (Ubah tipe partisi ke: `EFI System`)
> * `root` = **44.8G** (Ubah tipe partisi ke: `Linux Filesystem`)

```
lsblk
```
List Block Devices*. Digunakan untuk melihat tabel struktur seluruh media penyimpanan saat ini guna memastikan perubahan partisi telah sukses diterapkan.

---

## 3. Enkripsi dengan LUKS

Tahap mengamankan data dengan mengenkripsi partisi root secara penuh agar data tidak bisa diakses secara ilegal jika media penyimpanan hilang/dicuri.

```
cryptsetup luksFormat /dev/(partisi root)
```
Menginisialisasi dan memformat partisi root fisik menggunakan standar keamanan enkripsi **LUKS** (*Linux Unified Key Setup*). Anda akan diminta membuat *passphrase* (password dekripsi).
```
cryptsetup luksOpen /dev/(partisi root) (nama pemetaan)
```
 Membuka partisi terenkripsi menggunakan password yang telah dibuat, lalu memetakannya menjadi virtual device baru di direktori `/dev/mapper/(nama pemetaan)` agar bisa diolah.

---

## 4. Konfigurasi LVM (Logical Volume Manager)

LVM digunakan untuk membagi partisi terenkripsi yang sudah dibuka menjadi beberapa "partisi virtual" (Logical Volume) secara fleksibel.

## Menyiapkan Physical Volume & Volume Group
```
pvcreate /dev/mapper/(nama pemetaan)
```
Mengubah partisi terenkripsi (yang sudah dibuka) menjadi *Physical Volume* (PV) sebagai fondasi dasar bagi LVM.
```
vgcreate (nama grup) /dev/mapper/(nama pemetaan)
```
Membuat wadah besar bernama *Volume Group* (VG) di atas PV tersebut agar ruangnya bisa dibagi-bagi kembali menjadi beberapa volume anak.
```
lsblk
```
Memeriksa peta *block device* untuk memastikan struktur hierarki LVM awal telah terbentuk.

## Membuat & Memasang Logical Volume Root & Boot
```
lvcreate -L (G|M) (nama grup) -n root
```
Membuat *Logical Volume* (LV) baru untuk ruang sistem utama (`root`) dengan kapasitas tertentu (misal: `-L 20G`) di dalam Volume Group.
```
mkfs.ext4 /dev/(nama grup)/root
```
Memformat volume LVM `root` menggunakan sistem berkas standar Linux **ext4**.
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
```
Memformat partisi boot fisik dengan sistem berkas **FAT32** (wajib bagi UEFI) dan memberi nama label "BOOT".
```
mount /dev/(nama grup)/root /mnt
```
Mengaitkan (*mount*) sistem berkas root kustom ke direktori sementara `/mnt` pada live USB agar instalasi file sistem operasi bisa dimasukkan ke sana.
```
mount --mkdir -o uid=0,gid=0 /dev/(partisi boot) /mnt/boot
```
Membuat folder `/mnt/boot` sekaligus mengaitkan partisi EFI ke sana dengan hak akses (masking) yang sangat ketat demi alasan keamanan kredensial boot.

## Membuat Logical Volume Tambahan (*Server Hardening*)
Perintah di bawah ini memisahkan direktori kritis server menjadi volume tersendiri. Tujuannya agar jika salah satu sektor (seperti file Log) penuh, sistem utama (`root`) tidak ikut macet (*crash*).

```
lvcreate -L (G|M) (nama grup) -n vars`** → Alokasi ruang untuk data dinamis (`/var`).
lvcreate -L (G|M) (nama grup) -n vlog`** → Alokasi khusus untuk log sistem (`/var/log`).
lvcreate -L (G|M) (nama grup) -n vaud`** → Alokasi khusus untuk log audit keamanan (`/var/log/audit`).
lvcreate -L (G|M) (nama grup) -n vtmp`** → Alokasi khusus berkas sementara log (`/var/log/tmp`).
lvcreate -L (G|M) (nama grup) -n home`** → Alokasi ruang untuk berkas personal pengguna (`/home`).
lvcreate -L (G|M) (nama grup) -n podman`** → Alokasi ruang khusus untuk container data engine (`/var/lib/container`).
```
---

## 5. Format Partisi LVM

```
mkfs.ext4 /dev/(nama grup)/(nama_lv)
```
Memformat masing-masing *Logical Volume* kustom yang baru dibuat (`var`, `vlog`, `vaud`, `home`, `podman`) menggunakan format berkas **ext4** agar siap diisi data.

---

## 6. Mounting Partisi

Kaitkan semua Logical Volume LVM ke dalam struktur direktori instalasi utama (`/mnt/...`).

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/log/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/podman /mnt/var/lib/container
```

Membuat direktori sekaligus mengaitkan volume dengan opsi keamanan tinggi (*hardening*):
  * `nodev`: Melarang pembuatan berkas karakter atau blok khusus di volume tersebut.
  * `nosuid`: Mengabaikan bit SUID (mencegah manipulasi hak akses root biner).
  * `noexec`: Melarang eksekusi biner langsung pada direktori terkait (diterapkan pada folder log dan data).
```
lsblk
```
Memastikan seluruh struktur pemetaan mount poin di atas telah terpasang dengan benar tanpa ada yang terlewat.

---

## 7. Install Packages

```
pacstrap /mnt linux-hardened linux-hardened-headers lvm2 mkinitcpio linux-firmware-intel linux-firmware-realtek linux-firmware-atheros git neovim less intel-ucode firewalld pacman sudo iwd bash iputils iproute2 podman
```
Memasang (*install*) paket-paket dasar dari repositori ke dalam direktori `/mnt` (calon OS baru). 

> Paket yang dipasang meliputi kernel keamanan tingkat tinggi (`linux-hardened`), driver microcode prosesor, perkakas jaringan (`iwd`, `iproute2`), firewall (`firewalld`), container engine (`podman`), perkakas LVM (`lvm2`), dan teks editor (`neovim`).

---

## 8. File System Table (fstab)

```
genfstab -U /mnt > /mnt/etc/fstab
```
Mendeteksi seluruh partisi yang sedang di-*mount* saat ini dan membuat file konfigurasi `/etc/fstab` otomatis menggunakan kode UUID unik. File ini mendikte sistem partisi mana saja yang wajib dimuat otomatis saat komputer dihidupkan.
```
cat /mnt/etc/fstab
```
Menampilkan isi file fstab untuk memastikan tidak ada kesalahan pemetaan parameter keamanan atau UUID.

---

## 9. Membuat Hostname, Set Localtime, dan Locale
```
arch-chroot /mnt
```
Mengubah direktori utama (*root session*) terminal live USB ke dalam direktori `/mnt`. Mulai detik ini, setiap perintah yang diketik dijalankan langsung di dalam sistem operasi baru.

### Konfigurasi Nama Sistem & Waktu
```
nvim /etc/hostname
```
Membuka teks editor Neovim untuk menuliskan nama komputer (*hostname*) pada sistem server.
```
cat /etc/hostname
```
Memeriksa kembali isi file hostname.
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
Membuat *symbolic link* (jalan pintas) untuk menyetel wilayah zona waktu sistem ke Waktu Indonesia Barat (WIB).
```
hwclock --systohc
```
Sinkronisasi catatan waktu sistem Linux ke jam fisik (hardware/BIOS) pada motherboard komputer.

### Konfigurasi Bahasa (Locale)
```
nvim /etc/locale.gen
```
Membuka daftar pustaka bahasa untuk mengaktifkan jenis bahasa yang didukung oleh sistem (Hapus tanda `#` pada `en_US.UTF-8`).
```
locale-gen
```
Membuat kompilasi file lokalisasi bahasa berdasarkan pilihan di langkah sebelumnya.
```
locale > /etc/locale.conf`
```
Mengekspor variabel pengaturan bahasa dasar saat ini ke berkas konfigurasi sistem.
```
nvim /etc/locale.conf
```
Mengedit file konfigurasi agar menggunakan parameter standar bahasa internasional (`LANG=en_US.UTF-8` dan `LC_ALL=en_US.UTF-8`).

---

## 10. Home User Directory

```
mkdir -p /home/user
```
Membuat struktur folder direktori rumah untuk pengguna baru non-root.
```
useradd -d /home/user (nama user)
```
Mendaftarkan akun pengguna (*user*) baru ke dalam sistem OS dan mengarahkan direktori rumahnya ke folder yang telah dibuat.
```
passwd
```
Membuat password keamanan untuk akun administrator tertinggi (**root**).
```
chown -R (nama user):(nama user) /home/user
```
Mengubah hak kepemilikan (*ownership*) folder `/home/user` agar menjadi hak penuh milik user baru tersebut.
```
passwd (nama user)
```
Membuat password keamanan untuk akun pengguna non-root yang baru dibuat.
```
echo '(nama user) ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/(nama user)
```
Memasukkan hak khusus (akses administratif) ke direktori sudoers agar user tersebut bisa mengeksekusi perintah admin menggunakan awalan kata `sudo`.
```
cat /etc/sudoers.d/(nama user)
```
Memeriksa isi file konfigurasi hak akses sudo untuk memastikan kebenaran sintaks teks.

---

## 11. Konfigurasi Bootloader (Unified Kernel Image - UKI)

Tahap penggabungan kernel, ramdisk (initramfs), microcode, dan instruksi boot menjadi sebuah berkas tunggal berformat `.efi` demi keamanan dan kecepatan proses booting.

### Konfigurasi Argumen Kernel
```
mkdir /etc/cmdline.d
```
Membuat direktori penampung baris parameter instruksi boot kernel.
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
Membuat file teks kosong baru untuk membagi jenis parameter.
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root])=[nama] root=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```
Memasukkan perintah otomatisasi agar saat booting, sistem langsung melacak UUID partisi LUKS, meminta password dekripsi, dan membuka LVM group.
```
nvim /etc/cmdline.d/06-misc.conf
```
Mengisi file tambahan dengan instruksi `rw` agar sistem berkas di-*mount* dalam mode *Read-Write* (bisa dibaca dan ditulis) sejak awal boot.

### Konfigurasi Pemicu Ramdisk (mkinitcpio)
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
Memindahkan lokasi file konfigurasi pemicu kernel utama ke sub-direktori kustom agar manajemen berkas lebih rapi.
```
nvim /etc/mkinitcpio.d/default.conf
```
Mengonfigurasi baris susunan `HOOKS`. Menambahkan modul `sd-encrypt` dan `lvm2` sangat krusial agar sistem dapat mengenali enkripsi dan LVM sewaktu komputer pertama kali dinyalakan.

### Mengatur Layout & Berkas UKI
```
cd /boot
```
Pindah ke direktori boot sistem.
```
mkdir efi kernel
```
 Membuat struktur folder rapi untuk memisahkan file mentah sistem dengan file eksekusi EFI.
```
mv vmlinuz-linux-hardened intel-ucode.img kernel/
```
Memindahkan file kernel utama Linux Hardened dan file microcode prosesor ke dalam satu folder internal.
```
rm -fr initramfs-linux-hardened.img
```
Menghapus ramdisk lama bawaan standar, karena perannya akan digantikan penuh oleh kesatuan sistem berkas UKI yang baru.
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
Mengubah berkas cetak biru (*preset*) pembuatan boot agar hasil akhirnya langsung dikompilasi menjadi satu file berkas mandiri berformat `.efi` ke folder `/boot/efi/linux/arch-linux-hardened.efi`.
```
mkinitcpio -P
```
Menjalankan eksekusi pembuatan ulang seluruh skema image booting sistem operasi berdasarkan seluruh konfigurasi baru yang telah diatur.
```
ls
```

---

## 12. Systemd Boot

```
bootctl --path=/boot install
```
Memasang biner bootloader internal milik systemd (**systemd-boot**) langsung ke dalam sektor partisi EFI motherboard komputer Anda.
```
lsblk -o name,uuid
```
Mencocokkan kembali nama partisi dan nomor identitas UUID uniknya.
```
cat /etc/cmdline.d/01-boot.conf
```
Memastikan argumen pembaca luks tidak mengalami kesalahan ketik kode UUID.
```
ls efi/linux
```
Memeriksa apakah berkas UKI biner (`arch-linux-hardened.efi`) sudah sukses tercipta di folder bootloader.
```
bootctl status
```
Menampilkan informasi serta status kelayakan bootloader untuk memastikan sistem mengenali target OS baru saat dinyalakan nanti.

---

## 13. Booting

Langkah akhir untuk menyelesaikan proses instalasi dan masuk ke sistem utama.
```
exit
```
Keluar dari sesi virtual OS baru (`arch-chroot`) dan kembali ke terminal utama Live USB.
```
umount -R /mnt
```
Melepas kaitan (*unmount*) seluruh direktori dan sub-direktori di bawah folder `/mnt` secara aman agar tidak ada data yang rusak atau menggantung di memori RAM.
``` 
reboot
```
Memerintahkan komputer untuk melakukan muat ulang (*restart*). Sekarang Anda dapat mencabut flashdisk Live USB dan bersiap masuk ke dalam OS Server baru Anda.
