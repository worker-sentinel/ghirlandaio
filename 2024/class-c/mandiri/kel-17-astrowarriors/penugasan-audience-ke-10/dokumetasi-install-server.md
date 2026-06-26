# Instalasi Server, Setup Firewall, dan Disable Module Kernel
# Install Server
## 1. Membagi Partisi
```bash
cfdisk /dev/nvme0n1
```
```bash
nvme0n1p6 = 3G EFI FILE SYSTEM
```
Sisanya untuk Linux File Sytem
## 2. Mengecek Partisi
```bash
lsblk
```
## 3. Menghubungkan ke Wifi
```bash
iwctl
```
## 4. Konfigurasi LUKS
```bash
cryptsetup luksFormat /dev/nvme0n1p7
```
```bash
cryptsetup luksOpen /dev/nvme0n1p7 proc
```
## 5. Konfigurasi LVM
Membuat Physical Volume
```bash
pvcreate /dev/mapper/proc
```
Membuat Volume Grup
```bash
vgcreate saw /dev/mapper/proc
```
Membuat Logical Volume
```bash
lvcreate -L 8G saw -n root
lvcreate -L 5G saw -n vars
lvcreate -L 1G saw -n vlog
lvcreate -L 1G saw -n vaud
lvcreate -L 1.5G saw -n vtmp
lvcreate -l50%FREE saw -n home
lvcreate -l100%FREE saw -n podman
```
## 6. Membuat Filesystem
```bash
mkfs.ext4 /dev/proc/root
mkfs.vfat -F32 /dev/nvme0n1p6
mkfs.ext4 /dev/proc/vars
mkfs.ext4 /dev/proc/vlog
mkfs.ext4 /dev/proc/vaud
mkfs.ext4 /dev/proc/vtmp
mkfs.ext4 /dev/proc/home
mkfs.ext4 /dev/proc/podman
```
## 7. Mount Filesystem
```bash
mount /dev/saw/root
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,relatime /dev/saw/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/podman /mnt/var/lib/containers
```
## 8. Install Package
```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers Linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
## 9. Genfstab
```bash
genfstab -U /mnt > /mnt/etc/fstab
```
## 10. Mounting RAM
```bash
echo "tmpfs /tmp tmpfs deafults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```bash
arch-chroot /mnt
```
kalau berhasil tulisan root yang berwarna merah akan berubah menjadi putih
## 11. Konfigurasi System
Hostname
```bash
echo (nama computer) > /etc/hostname
```
Timezone
```bash
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
```bash
nvim /etc/locale.gen
```
hapus # en_US<br>
Locale
```bash
locale-gen
```
```bash
locale > /etc/locale.conf
```
```bash
nvim /etc/locale.conf
```
## 12. Konfigurasi User
```bash
useradd -m (nama user)
```
```bash
passwd (nama user)
```
```bash
echo "(nama user) ALL=(ALL:ALL) ALL" > /etc/sudoers.d/(nama user)
```
Uji coba
```bash
su (nama user)
```
Jika berhasil nama root akan berubah menjadi nama user
```bash
sudo su
```
Masukkan Password
## 13. Konfigurasi Kernel
Membuat direktori
```bash
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-nisc.conf}
```
Cek
```bash
lsblk
```
Konfigurasi boot
```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=proc root=/dev/saw/root" > /etc/cmdline.d/01-boot.conf
```
Memastikan isi file
```bash
cat /etc/cmdline.d/01-boot.conf
echo rw > /etc/cmdline.d/02-nisc.conf
```
## 14. Konfigurasi Initramfs
```bash
nvim /etc/mkinitcpio.conf
```
Masuk ke HOOKS, tambahkan
```bash
sd-encypt lvm2
```
Edit preset
```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```
## 15. Install Sytem.d Boot
```bash
bootctl --path=/boot install
```
```bash
mkinitcpio -P
```
## 16. Mengaktifkan Service System
```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
```
Keluar dari chroot
```bash
exit
reboot
```
## 17. Konfigurasi Jaringan Setelah Instalasi
```bash
nvim /etc/resolv.conf
```
Tambahkan
```bash
nameserver 8.8.8.8
nameserver 1.1.1.1
```
Restart systemd
```bash
systemctl restart systemd-networkd
systemctl restart systemd-resolved
```
Konfigurasi iwd
```bash
nvim /etc/iwd/main.conf
```
Isi file
```bash
[general]
EnableNetworkConfiguration=true
```
Restart iwd
```bash
systemctl restart iwd
```
Uji Koneksi
```bash
ping 8.8.8.8
```
## 18. Konfigurasi Firewall
Mengecek status firewall
```bash
systemctl status firewalld
```
```bash
firewall-cmd --info-zone=drop
firewall-cmd --info-zone=block
firewall-cmd --info-zone=public
firewall-cmd --info-zone=external
firewall-cmd --info-zone=internal
firewall-cmd --info-zone=dmz
firewall-cmd --info-zone=work
firewall-cmd --info-zone= home
firewall-cmd --info-zone= trusted
```
Hapus yang tidak digunakan
```bash
firewall-cmd --info-zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload

firewall-cmd --info-zone=external --remove-service=ssh --permanent
firewall-cmd --reload

firewall-cmd --info-zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload

firewall-cmd --info-zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload

firewall-cmd --info-zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload

firewall-cmd --info-zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```
## 19.  Disable Module Kernel
```bash
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep overlayfs
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep afs
lsmod | grep ceph
lsmod | grep cifs
lsmod | grep exfat 
lsmod | grep ext 
lsmod | grep fat 
lsmod | grep fscache 
lsmod | grep fuse 
lsmod | grep gfs2 
lsmod | grep nfs_common 
lsmod | grep nfsd 
lsmod | grep smbfs_common 
```
Selain fat, jika muncul maka silahkan di disable<br>
Jika modul ditemukan, buat konfigurasi
```bash
nvim /etc/modprobe.d/disable-module.conf
```
Masukkan
```bash
install usb-storage /bin/false
blacklist usb-storage
```
Perbarui initramfs
```bash
mkinitcpio -P
```









