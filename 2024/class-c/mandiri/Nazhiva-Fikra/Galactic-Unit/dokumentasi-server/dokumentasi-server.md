# Install OS 
## Cek disk
```
lsblk
```

## Koneksi WiFi
```
iwctl
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect "nama_wifi"
```
```
exit
```
```
ping 8.8.8.8
```

## Membagi partisi disk
```
cfdisk /dev/nvme0n1
```

## Setup LUKS 
```
cryptsetup LuksFormat /dev/nvme0n1p12
```
```
cryptsetup LuksOpen /dev/nvme0n1p12 Server
```

## Setup LVM
```
pvcreate /dev/mapper/Server
```
```
vgcreate K23 /dev/mapper/Server
```
```
lvcreate -L 12G K23 -n root
```
```
lvcreate -L 12G K23 -n vars
```
```
lvcreate -L 1G K23 -n vlog
```
```
lvcreate -L 1G K23 -n vtmp
```
```
lvcreate -L 1G K23 -n vaud
```
```
lvcreate -L 7G K23 -n home
```

## Format partisi
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p11
```
```
mkfs.ext4 /dev/K23/root
```
```
mkfs.ext4 /dev/K23/vars
```
```
mkfs.ext4 /dev/K23/vtmp
```
```
mkfs.ext4 /dev/K23/vlog
```
```
mkfs.ext4 /dev/K23/vaud
```
```
mkfs.ext4 /dev/K23/home
```

## Mount partisi
```
mount /dev/K23/root /mnt
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p11 /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/K23/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/K23/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/K23/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/K23/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nosuid,relatime /dev/K23/home /mnt/home
```

## Install sistem
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

# Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
```
cat /mnt/etc/fstab
```

```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Masuk chroot
```
arch-chroot /mnt
```

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## Edit locale
```
nvim /etc/locale.gen
```

uncomment en_US.UTF-8 dan id_ID.UTF-8

```
locale-gen
```
```
nvim /etc/locale.conf
```
tulis
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

## Buat user
```
useradd -m F123
```
```
passwd F123
```
```
echo "F123 ALL=(ALL:ALL) ALL" > /etc/sudoers.d/F123
```

## Setup cmdline untuk LUKS+LVM
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/01-boot.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p12)=Server root=/dev/K23/root" > /etc/cmdline.d/01-boot.conf
```

## Edit mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```

## Install bootloader
```
bootctl --path=/mnt/boot install
```

## Generate initramfs
```
mkinitcpio -P
```

## Reboot
```
exit
```
```
reboot
```

# Disable Modul
## 1. Masuk Sebagai Root

```
sudo su
```

Berpindah ke user root untuk melakukan konfigurasi sistem.

---

## 2. Memeriksa Modul FreeVXFS

```
lsmod | grep freevxfs
```

Memeriksa apakah modul `freevxfs` sedang dimuat oleh kernel.

---

## 3. Memeriksa Modul HFS

```
lsmod | grep hfs
```

Memeriksa apakah modul `hfs` sedang digunakan.

---

## 4. Memeriksa Modul HFS+

```
lsmod | grep hfsplus
```

Memeriksa keberadaan modul `hfsplus`.

---

## 5. Memeriksa Modul JFFS2

```
lsmod | grep jffs2
```

Memeriksa apakah modul `jffs2` aktif.

---

## 6. Memeriksa Modul SquashFS

```
lsmod | grep squashfs
```

Memeriksa status modul `squashfs`.

---

## 7. Memeriksa Modul UDF

```
lsmod | grep udf
```

Memeriksa apakah modul `udf` sedang dimuat.

---

## 8. Memeriksa Modul USB Storage

```
lsmod | grep usb-storage
```

Memeriksa keberadaan modul penyimpanan USB.

---

## 9. Mengedit File Konfigurasi Modul

```
nvim /etc/modprobe.d/disable-module.conf
```

Membuka file konfigurasi untuk menonaktifkan modul kernel yang tidak diperlukan.

```text
blacklist freevxfs
install freevxfs /bin/false

blacklist hfs
install hfs /bin/false

blacklist hfsplus
install hfsplus /bin/false

blacklist jffs2
install jffs2 /bin/false

blacklist udf
install udf /bin/false

blacklist usb-storage
install usb-storage /bin/false
```

---

## 10. Memeriksa modul kernel 
lsmod | grep fat
Modul kernel untuk sistem berkas FAT/vfat

nvim /etc/modprobe.d/01-disable-module.conf
untuk membuka text editor Neovim





## 11. Regenerasi Initramfs

```
mkinitcpio -P
```

Membangun ulang initramfs agar perubahan konfigurasi modul diterapkan pada proses boot berikutnya.

---

## 11. Keluar dari Sesi Root

```
exit
```

Keluar dari user root dan kembali ke user biasa.

# Firewall
## 1. Mengaktifkan Firewalld


systemctl enable firewalld
systemctl status firewalld
```

Mengaktifkan Firewalld dan memeriksa status service.

---

## 2. Memeriksa Zone Drop


firewall-cmd --info-zone=drop
```

Menampilkan informasi konfigurasi zone `drop`.

---

## 3. Memeriksa Zone Block


firewall-cmd --info-zone=block
```

Menampilkan informasi konfigurasi zone `block`.

---

## 4. Memeriksa Zone Public


firewall-cmd --info-zone=public
```

Menampilkan konfigurasi zone `public`.

---

## 5. Menghapus Service DHCPv6 Client dari Zone Public


firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload
```

Menghapus service `dhcpv6-client` dari zone `public` dan menerapkan perubahan.

---

## 6. Memeriksa Zone External


firewall-cmd --info-zone=external
```

Menampilkan konfigurasi zone `external`.

---

## 7. Menghapus Service SSH dari Zone External

```bash
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `external`.

---

## 8. Memeriksa Zone Internal

```
firewall-cmd --info-zone=internal
```

Menampilkan konfigurasi zone `internal`.

---

## 9. Menghapus Service pada Zone Internal

```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `internal`.

---

## 10. Memeriksa Zone DMZ

```
firewall-cmd --info-zone=dmz
```

Menampilkan konfigurasi zone `dmz`.

---

## 11. Menghapus Service SSH dari Zone DMZ

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `dmz`.

---

## 12. Memeriksa Zone Work

```
firewall-cmd --info-zone=work
```

Menampilkan konfigurasi zone `work`.

---

## 13. Menghapus Service pada Zone Work

```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `work`.

---

## 14. Memeriksa Zone Home

```bash
firewall-cmd --info-zone=home
```

Menampilkan konfigurasi zone `home`.

---

## 15. Menghapus Service pada Zone Home

```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `home`.

































