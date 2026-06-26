# Dokumentasi Install OS Admin
## Nama: Amelia Salsabila Santoso
## NIM: 12402051030089
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.

### Konfigurasi Koneksi Jaringan:
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect nama_wifi
```
### Pengujian koneksi jaringan
```
ping 1.1.1.1
```
```
ping 8.8.8.8
```
### Melakukan rekaman dengan asciinema
```
asciinema rec [nama_file].cast
```
### Melihat Partisi
```
lsblk
```
### Membagi partisi dengan EFI System dan Linux Filesystem 
```
cfdisk /dev/sda
```
### Mengenkripsi Partisi dengan LUKS
```
cryptsetup luksFormat /dev/sda7
```
### Membuka Partisi Enkripsi
```
cryptsetup luksOpen /dev/sda7 admin
```
### Membuat LVM
```
pvcreate /dev/mapper/admin
```
### Membuat volume group
```
vgcreate team /dev/mapper/admin
```
### Membuat LV
```bash
lvcreate -L 25G team -n root
lvcreate -L 15G team -n vars
lvcreate -L 5G team -n vtmp
lvcreate -L 5G team -n vlog
lvcreate -L 5G team -n vaud
lvcreate -L 60G team -n home
```

### Format Filesystem
```
mkfs.vfat -F32 -n BOOT /dev/sda6
```
```
mkfs.ext4 -b 4096 /dev/team/root
```
```
mkfs.ext4 -b 4096 /dev/team/vars
```
```
mkfs.ext4 -b 4096 /dev/team/vtmp
```
```
mkfs.ext4 -b 4096 /dev/team/vlog
```
```
mkfs.ext4 -b 4096 /dev/team/vaud
```
```
mkfs.ext4 -b 4096 /dev/team/home
```
```
mkfs.ext4 -b 4096 /dev/team/podman
```
### Format Partisi Boot
```
mkfs.vfat -F32 -n BOOT /dev/sda6
```
### Mount root
```
mount /dev/team/root /mnt
```
### Mount partisi lain
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda6 /mnt/boot
```

```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/team/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,relatime /dev/team/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/podman /mnt/var/lib/containers
```

### Pacstrap
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose
```
### Membuat fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Copy Konfigurasi Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
### Menambah tmpfs
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
### Melihat file fstab
```
cat /mnt/etc/fstab
```

### Masuk ke sistem baru
```
arch-chroot /mnt
```
### Mengatur Timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
### Sinkronisasi Jam
```
hwclock --systohc
```
### Mengatur locale
```
nvim /etc/locale.gen
```
```
en_US.UTF-8 UTF-8
```
```
en_US ISO-8859-1
```
```
locale.gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
LANG=en_US.UTF-8
```
```
LC_ALL=en_US.UTF-8
```
### Membuat User
```
useradd -m atmin
```
```
passwd atmin
```
### Memberikan Hak Sudo
```
echo "atmin ALL=(ALL:ALL) ALL" > /etc/sudoers.d/atmin
```
### Konfigurasi Kernel Command Line
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda7)=admin root=/dev/team/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
### Konfigurasi mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
```
tambahkan kalimat sesudah "block" "sd-encrypt lvm2"
##   NOTE: If you have /usr on a separate partition, you MUST include the
#    usr and fsck hooks.
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block [sd-encrypt lvm2] filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
ALL_config="/etc/mkinitcpio.conf" 
ALL_kver="/boot/vmlinuz-linux-lts"   
ALL_kerneldest="/boot/vmlinuz-linux-lt

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/boot/efi/Linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"
```
### Menginstall Bootctl
```
bootctl --path=/boot install
```
### Generate Initramfs
```
mkinitcpio -P
```
### Instalasi Desktop Environment
```
pacman -S sddm xfce4 pipewire pipewire-pulse pipewire-jack pipewire-alsa network-manager-applet networkmanager firefox
```

### Mengaktifkan Network
```
systemctl enable firewalld
```
```
systemctl enable NetworkManager 
```
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```
### Install Podman-Compose
```
pacman -S podman podman-compose
```

### Booting
```
exit
```
```
umount -R /mnt
```
### Mematikan asciinema
```
ctrl+d
```
```
asciinema rec [nama_file].cast
```
### Reboot
```
Reboot
```
