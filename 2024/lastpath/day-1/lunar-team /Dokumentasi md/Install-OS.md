# Instalasi Arch Linux — Server LUKSonLVM

---

## 1. Bagi Partisi (LVM)

```bash
lsblk
pvcreate /dev/[nama partisi]
vgcreate [nama grup] /dev/[nama partisi]

lvcreate -L [size G|M] [nama grup] -n root
lvcreate -L [size G|M] [nama grup] -n vars
lvcreate -L [size G|M] [nama grup] -n vlog
lvcreate -L [size G|M] [nama grup] -n vaud
lvcreate -L [size G|M] [nama grup] -n vtmp
lvcreate -L [size G|M] [nama grup] -n home
lvcreate -l 50%FREE [nama grup] -n podman
lvcreate -l 50%FREE [nama grup] -n [nama home untuk internal]

lsblk
```

> LUKS pada LVM `home` ada **dua**: eksternal dan internal.

---

## 2. Format Partisi

```bash
mkfs.ext4 /dev/[nama grup]/root
mkfs.ext4 /dev/[nama grup]/vars
mkfs.ext4 /dev/[nama grup]/vlog
mkfs.ext4 /dev/[nama grup]/vaud
mkfs.ext4 /dev/[nama grup]/vtmp
mkfs.ext4 /dev/[nama grup]/home
mkfs.ext4 /dev/[nama grup]/podman

# Kalau lupa nama partisi:
lsblk -o name,fstype

mkfs.vfat -F32 /dev/[nama partisi boot]
```

---

## 3. Mounting

```bash
mount /dev/[nama grup]/root /mnt

mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```

> `home` untuk **internal** tidak perlu dimounting di sini, karena nanti akan dimounting otomatis menggunakan aplikasi `pam_mount`.

### Setup LUKS untuk Home Internal

```bash
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
# Konfirmasi: YES
# Buat password — password ini HARUS SAMA dengan password user nanti
```

---

## 4. Install Package

```bash
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware \
  mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep \
  podman podman-compose asciinema
```

---

## 5. After Install

```bash
genfstab -U /mnt > /mnt/etc/fstab
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab

cp /etc/systemd/network/* /mnt/etc/systemd/network

arch-chroot /mnt

echo [hostname] > /etc/hostname
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc

nvim /etc/locale.gen
# Cari baris EN_US, hapus tanda pagar (#) di kedua barisnya

locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf

pacman -S pam_mount

lsblk
```

---

## 6. Setup Home Internal

```bash
cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
# Masukkan password yang sudah dibuat sebelumnya

mkfs.ext4 /dev/mapper/[device name]

mkdir /home/[nama folder]
useradd -d /home/[nama folder] [nama user]

passwd [nama user]
# Masukkan password — HARUS SAMA dengan password saat cryptsetup luksFormat

echo "[nama user] ALL=(ALL:ALL) ALL" > /etc/sudoers.d/[nama user]

chown -R [nama user]:[nama user] /home/[nama folder]

nvim /etc/security/pam_mount.conf.xml

nvim /etc/pam.d/system-login

```
---
<img width="987" height="491" alt="WhatsApp Image 2026-06-24 at 1 10 31 AM" src="https://github.com/user-attachments/assets/59d020d6-68fb-4748-bd56-c905f36323a9" />

---

---
<img width="976" height="676" alt="WhatsApp Image 2026-06-24 at 1 10 31 AM (1)" src="https://github.com/user-attachments/assets/14b5a86a-4ecb-4510-b2da-85d71d724818" />

---


## 7. Mkinitcpio

```bash
mkdir /etc/mkinitcpio.d
nvim /etc/cmdline.d/boot.conf
nvim /etc/mkinitcpio.conf
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

---

## 8. Install systemd-boot

```bash
bootctl --path=/boot install
mkinitcpio -P

systemctl enable systemd-networkd
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd

exit

umount -R

reebot
```

### Khusus untuk Lenovo

```bash
bootctl --path=/mnt/boot install
```

```bash
reboot
```
