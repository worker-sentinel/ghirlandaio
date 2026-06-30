# Dokumentasi Instalasi OS
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Salfa Firyal Hasanah
3. Fatma Ramadhani
4. Aditya Pangruwating Dhiyu
5. Fauzan Azhiimi
6. Ahmad Hafiz Baihaqi
## Persiapan

### 1. Merekam proses instalasi (opsional)

```bash
asciinema rec nama.cast
```

---

### 2. Menghubungkan ke Wi-Fi

Masuk ke utilitas iwd.

```bash
iwctl
```

Lihat daftar jaringan Wi-Fi.

```bash
station wlan0 get-networks
```

Hubungkan ke Wi-Fi.

```bash
station wlan0 connect "Nama WiFi"
```

Masukkan password Wi-Fi.

Keluar dari iwd.

```bash
exit
```

Pastikan koneksi internet sudah aktif.

```bash
ping 1.1.1.1
```

---

# Partisi Disk

Jalankan utilitas partisi.

```bash
cfdisk /dev/sda
```

Lakukan konfigurasi berikut:

- Hapus partisi Arch Linux lama (jika ada).
- Buat partisi EFI sebesar **2 GB**.
- Tipe partisi **EFI System**.
- Buat satu partisi lagi menggunakan sisa kapasitas disk.
- Tipe partisi **Linux Filesystem**.

Cek hasil partisi.

```bash
lsblk
```

---

# Enkripsi LUKS

Format partisi root menggunakan LUKS.

```bash
cryptsetup luksFormat /dev/<partisi-root>
```

Ketik

```text
YES
```

Lalu masukkan password LUKS.

Buka partisi yang telah dienkripsi.

```bash
cryptsetup luksOpen /dev/<partisi-root> nama_mapper
```

Masukkan password yang telah dibuat sebelumnya.

---

# Membuat LVM

Membuat Physical Volume.

```bash
pvcreate /dev/mapper/nama_mapper
```

Membuat Volume Group.

```bash
vgcreate nama_vg /dev/mapper/nama_mapper
```

---

# Membuat Logical Volume

Root

```bash
lvcreate -L 8G nama_vg -n root
```

Var

```bash
lvcreate -L 5G nama_vg -n vars
```

Var Log

```bash
lvcreate -L 1G nama_vg -n vlog
```

Var Log Audit

```bash
lvcreate -L 1G nama_vg -n vaud
```

Var Tmp

```bash
lvcreate -L 1G nama_vg -n vtmp
```

Home

```bash
lvcreate -L 1G nama_vg -n home
```

Podman

```bash
lvcreate -l100%FREE nama_vg -n podman
```

Periksa hasilnya.

```bash
lsblk
```

---

# Membuat Filesystem

EFI

```bash
mkfs.vfat -F32 -S4096 -n BOOT /dev/<partisi-boot>
```

Root

```bash
mkfs.ext4 /dev/nama_vg/root
```

Var

```bash
mkfs.ext4 /dev/nama_vg/vars
```

Var Log

```bash
mkfs.ext4 /dev/nama_vg/vlog
```

Var Log Audit

```bash
mkfs.ext4 /dev/nama_vg/vaud
```

Var Tmp

```bash
mkfs.ext4 /dev/nama_vg/vtmp
```

Home

```bash
mkfs.ext4 /dev/nama_vg/home
```

Podman

```bash
mkfs.ext4 /dev/nama_vg/podman
```

Periksa kembali.

```bash
lsblk
```

---

# Mount Filesystem

Mount root.

```bash
mount /dev/nama_vg/root /mnt
```

Mount EFI.

```bash
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/<partisi-boot> /mnt/boot
```

Mount `/var`.

```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/nama_vg/vars /mnt/var
```

Mount `/var/log`.

```bash
mount --mkdir -o rw,noexec,nosuid,relatime /dev/nama_vg/vlog /mnt/var/log
```

Mount `/var/log/audit`.

```bash
mount --mkdir -o rw,noexec,nosuid,relatime /dev/nama_vg/vaud /mnt/var/log/audit
```

Mount `/var/tmp`.

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama_vg/vtmp /mnt/var/tmp
```

Mount `/home`.

```bash
mount --mkdir -o rw,nosuid,relatime /dev/nama_vg/home /mnt/home
```

Mount direktori Podman.

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama_vg/podman /mnt/var/lib/containers
```

Verifikasi.

```bash
lsblk
```

---

# Instalasi Arch Linux

```bash
pacstrap /mnt intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman sddm asciinema networkmanager openssh
```

---

# Konfigurasi Awal

Generate fstab.

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

Salin konfigurasi jaringan.

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

Tambahkan tmpfs.

```bash
echo "tmpfs /tmp tmpfs defaults,nodev,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```

Masuk ke sistem baru.

```bash
arch-chroot /mnt
```

---

# Konfigurasi Sistem

Hostname.

```bash
echo nama-host > /etc/hostname
```

Timezone.

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Sinkronisasi hardware clock.

```bash
hwclock --systohc
```

---

# Locale

Edit locale.

```bash
nvim /etc/locale.gen
```

Hilangkan tanda `#` pada:

```text
en_US.UTF-8 UTF-8
```

Generate locale.

```bash
locale-gen
```

Buat locale.conf.

```bash
locale > /etc/locale.conf
```

Edit locale.

```bash
nvim /etc/locale.conf
```

Isi menjadi:

```text
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

---

# Membuat User

```bash
useradd -m nama_user
```

```bash
passwd nama_user
```

Tambahkan sudo.

```bash
echo "nama_user ALL=(ALL:ALL) ALL" > /etc/sudoers.d/nama_user
```

---

# Konfigurasi Kernel Command Line

```bash
mkdir /etc/cmdline.d
```

```bash
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

```bash
ls /etc/cmdline.d
```

```bash
lsblk
```

```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/<partisi-root>)=com root=/dev/nama_vg/root" > /etc/cmdline.d/01-boot.conf
```

```bash
cat /etc/cmdline.d/01-boot.conf
```

---

# Konfigurasi mkinitcpio

```bash
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```

Edit konfigurasi.

```bash
nvim /etc/mkinitcpio.d/default.conf
```

Ubah bagian HOOKS menjadi:

```text
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```

---

Edit preset.

```bash
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

Sesuaikan menjadi:

```text
ALL_config="/etc/mkinitcpio.d/default.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"
default_uki="/boot/efi/linux/arch-linux-hardened.efi"
```

---

# Konfigurasi Boot

```bash
cd /boot
```

```bash
mkdir kernel efi
```

```bash
mkdir efi/boot efi/systemd efi/linux
```

```bash
mv intel-ucode.img vmlinuz-linux-hardened kernel/
```

```bash
rm -f initramfs-linux-hardened.img
```

Install systemd-boot.

```bash
bootctl --path=/boot install
```

---

# Generate Initramfs

```bash
touch /etc/vconsole.conf
```

```bash
mkinitcpio -P
```

---

# Keluar dari chroot

```bash
exit
```

---

# Mengaktifkan Service

```bash
systemctl enable systemd-networkd
```

```bash
systemctl enable systemd-resolved
```

```bash
systemctl enable iwd
```

```bash
systemctl enable firewalld
```

```bash
systemctl enable sshd
```

---

# Selesai

Unmount seluruh filesystem.

```bash
umount -R /mnt
```

Keluar dari sesi instalasi.

```bash
exit
```

Hentikan rekaman Asciinema.

```bash
exit
```

Upload hasil rekaman.

```bash
asciinema upload nama.cast
```

Reboot sistem.

```bash
reboot
```

Saat komputer mulai menyala kembali:

- Lepaskan flashdisk installer.
- Tekan **F7** (atau tombol Boot Menu sesuai perangkat).
- Pilih **UEFI OS** untuk melakukan boot ke Arch Linux yang baru diinstal.
