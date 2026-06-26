# connect wifi

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
station wlan0 connect REAL
```
```
exit
```
```
ping 8.8.8.8
```

# PARTISI

## membagi partisi
```
cfdisk /dev/nvme0n1
```
### Sesuaikan Partisi
```
boot = 5 efi system
system = sisanya linux filesystem
```
`
# Setup Luks

```
cryptsetup luksFormat --sector-size 4090 /dev/nvme0n1p6
```
```
cryptsetup luksOpen /dev/partisi_systen oxva
```

```
pvcreate /dev/mapper/oxva
```
```
vgcreate xlim /dev/mapper/oxva
```

# Setup LVM

### Root
```
lvcreate -L 15G xlim -n root
```
```
mkfs.ext4 /dev/xlim/root
```
```
mount /dev/xlim/root /mnt
```
### boot
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p4
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p4 /mnt/boot
```
### vars
```
lvcreate -L 15G oxva -n vars
```
```
mkfs.ext4 /dev/oxva/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/oxva/vars /mnt/var
```
### tmp
```
lvcreate -L 5G oxva -n tmp
```
```
mkfs.ext4 /dev/oxva/vtemp
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/oxva/tmp /mnt/tmp
```
### vtemp
```
lvcreate -L 5G oxva -n vtemp
```
```
mkfs.ext4 /dev/oxva/vtemp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/oxva/vtemp /mnt/var/tmp
```
### vlog
```
lvcreate  -L 5G oxva -n vlog
```
```
mkfs.ext4 /dev/oxva/vlog
```
```
mount --mkdir -o rw,nodev,oxva,noexec,relatime /dev/oxva/vlog /mnt/var/log
```
### Vaud
```
lvcreate -L 5G oxva -n vaud
```
```
mkfs.ext4 /dev/oxva/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/oxva/vaud /mnt/var/log/audit
```
### dock
```
lvcreate -L 10G oxva -n dock
```
```
mkfs.ext4 /dev/oxva/dock
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/oxva/dock /mnt/var/lib/docker
```
### home
```
lvcreate -l70%FREE oxva -n home
```
```
mkfs.ext4 /dev/oxva/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/oxva/home /mnt/home
```
## install package
```
lspci
```
```
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema linux-firmware-realtek firewalld
```
### regist partisi
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
genfstab -U /mnt > /mnt/etc/fstab
```
### chroot
```
arch-chroot /mnt
```
### time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```

cari en us

```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
### user
```
useradd -m iqbal
```
```
passwd iqbal
```
```
echo "nama_user ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/nama_user
```
### kernel
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6)=xlim root=/dev/oxva/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
### mkinitcpio
```
cd /boot
```
```
mkdir kernel efi
```
```
cd efi
```
```
mkdir linux
```
```
cd ..
```
```
mv vmlinuz-* intel-* kernel
```
```
rm -fr initramfs-*
```
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```

tambahkan lvm2 dan sd-encrypt setelah sd-vconsole

```
nvim /etc/mkinitcpio.d/linuxx-lts-preset
```
sesuaikan

```
bootctl --path=/boot install
mkinitcpio -P
systemctl enable iwd
systemctl enable systemd-networkd.socket
systemctl enable systemd-resolved
```
### finish
```
exit
```
```
umount -R /mnt
```
```
reboot
```
