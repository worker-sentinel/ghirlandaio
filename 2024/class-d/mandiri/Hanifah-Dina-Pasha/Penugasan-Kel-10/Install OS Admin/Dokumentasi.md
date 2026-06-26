# Dokumentasi Install OS Admin

---
## Melihat partisi disk
```
lsblk
```
## Menghapus Volume Grup LVM lama
```
vgremove admin
```
## Membuat Physical Volume Baru
```
pvcreate /dev/(partisi)
```
## Membuat Volume Grup Baru 
```
vgcreate admin /dev/(partisi)
```
## Membuat Logical Volume 
```
> lvcreate -L 10G admin -n root
> lvcreate -L 10G admin -n vars
> lvcreate -L 10G admin -n vlog
> lvcreate -L 10G admin -n vtmp
> lvcreate -L 10G admin -n vaud
> lvcreate -L 10G admin -n home
> lvcreate -L 10G admin -n podman
```
## Memformat filesystem
```
> mkfs.vfat -F32 -n BOOT /dev/(partisi)
> mkfs.ext4 /dev/admin/root
> mkfs.ext4 /dev/admin/vars
> mkfs.ext4 /dev/admin/vlog
> mkfs.ext4 /dev/admin/vtmp
> mkfs.ext4 /dev/admin/vaud
> mkfs.ext4 /dev/admin/home
> mkfs.ext4 /dev/admin/podman
```
## Mounting
```
> mount /dev/admin/root /mnt
> mount --mkdir -o uid=,gid=0,fmask=0077,dmask=0077 /dev/(partisi) /mnt/boot
> mount --mkdir -o rw,nodev,nosuid,relatime /dev/admin/vars /mnt/var
> mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/admin/vtmp /mnt/var/tmp
> mount --mkdir -o rw,nosuid,noexec,relatime /dev/admn/vlog /mnt/var/log
> mount --mkdir -o rw,nosuid,noexec,relatime /dev/admin/vaud /mnt/var/log/audit
> mount --mkdir -o rw,nosuid,relatime /dev/admin/home /mnt/home
> mount --mkdir -o rw,nosuid,noexec,relatime /dev/admin/podman /mnt/var/lib/containers
```
## Membuat volume enkripsi
```
> lvcreate -l50%free admin -n ADMIN
> cryptsetup luksFormat /dev/admin/ADMIN
```
## Install sistem operasi
```
pacstrap /mnt base inter-ucode linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose asciinema firefox
```
## fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Mengcopy network konfigurasi
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Memformat tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
## Melihat file konfigurasi
```
cat /mnt/etc/fstab
```
## Pindah ke lingkungan sistem yang baru di install
```
arch-chroot /mnt
```
## Konfigurasi zona waktu
```
> ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
> hwclock --systohc
```
## Konfigurasi bahasa sistem
```
nvim /etc/locale.gen
```
> cari en_US, lalu hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
```
locale-gen 
```
locale > /etc/locale.conf
```
nvim /etc/locale.conf
```
> ganti menjadi LANG=en_US.UTF-8 dan paling bawah tambahkan LC_ALL=en_US.UTF-8
```
> useradd -m (nama user)
> passwd (nama user)
> echo "nama user ALL=(ALL:ALL) ALL" > etc/sudoers.d/nama user
```
## Kernel
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root])=nama user=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
```
nvim /etc/mkinitcpio.conf
```
> diganti menjadi HOOKS=(base udev microcode modconf block sdm-crypt lvm2 filesystem fsck)


## Konfigurasi Initramfs
```
nvim /etc/mkinitcpio.d/Linux-lts.preset
```
> hilangkan pagar, kemudian ganti menjadi ALL_config="/etc/mkinitcpio.d/default.com"
> ganti menjadi ALL_kver="/boot/kernel/vmlinuz-linux-lts"
> hilangkan # pada ALL_kerneldest+"/boot/kernel/vmlinuz-linux-lts"
> tambahkan #default_image="/boot/initramfs-linux-lts.img"
> hilangkan efi huruf kecil dan ganti menjadi default_uki="/boot/EFI/Linux/arch-Linux-lts.efi"


## Instalasi Bootloader
```
bootctl --path=/boot install
```
## Membuat initramfs
```
mkinitcpio -P
```
## Mengaktifkan dan menjalankan jaringan 
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```

## Keluar dari terminal
```
exit
```
