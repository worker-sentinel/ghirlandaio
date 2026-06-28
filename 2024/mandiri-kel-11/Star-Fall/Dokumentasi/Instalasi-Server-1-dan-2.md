# Instalasi OS untuk server 
## Membagi partisi
```
lsblk
```
```
pvcreate /dev/(nama partisi)
```
```
vgcreate (nama volume grup) /dev/(nama partisi)
```
```
lvcreate -L (size G | M) (nama volume grup) -n root
```
```
lvcreate -L (size G | M) (nama volume grup) -n vars
```
```
lvcreate -L (size G | M) (nama volume grup) -n vlog
```
```
lvcreate -L (size G | M) (nama volume grup) -n vaud
```
```
lvcreate -L (size G | M) (nama volume grup) -n vtmp
```
```
lvcreate -L (size G | M) (nama volume grup) -n home
```
```
lvcreate -l50%FREE (nama volume grup) -n podman
```
```
lvcreate -l50%FREE (nama volume grup) -n (nama folder home)
```
> cek kembali dengan
```
lsblk
```
> Pada Luks on Lvm, terdapat 2 partisi home yang digunakan untuk eksternal dan internal

## Format Partisi
```
mkfs.ext4 /dev/(nama volume grup)/root
```
```
mkfs.ext4 /dev/(nama volume grup)vars
```
```
mkfs.ext4 /dev/(nama volume grup)/vlog
```
```
mkfs.ext4 /dev/(nama volume grup)/vaud
```
```
mkfs.ext4 /dev/(nama volume grup)/vtmp
```
```
mkfs.ext4 /dev/(nama volume grup)/home
```
```
mkfs.ext4 /dev/(nama volume grup)/podman
```
```
mkfs.ext4 -F32 /dev/(nama partisi boot)
```

## Mounting Partisi
```
mount /dev/(nama volume grup) /root/mnt
```
```
mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/(nama partisi boot) /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/podman /mnt/var/lib/containers
```
> partisi home untuk internal tidak dimounting manual, karena akan termounting otomatis menggunakan pam_mount
cryptsetup luksFormat /dev(nama volume grup)/(nama partisi home internal)

## Install Package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firrewalld openssh grep podman podman-compose asciinema
```
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0  0" >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/networkd
```
```
arch-chroot /mnt
```
```
echo (hostname) > /etc/hostname
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen 
```
> cari en_US kemudian hapus hashtag
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
pacman -S pam_mount
```

## Setup Home Internal
```
cryptsetup luksOpen /dev/(nama grup)/(nama home internal) (device name) 
```
```
mkfs.ext4 /dev/mapper/(device name)
```
```
mkdir /home/(nama folder)
```
```
useradd -d /home/(nama folder) (nama user)
```
```
passwd (nama user)
```
```
echo “(nama user) ALL = (ALL:ALL)” > /etc/sudoers.d/(nama user)
```
```
chown -R (nama user):(nama user) /home/(nama folder)
```
```
nvim /etc/security/pam_mount.conf.xml 
```
```
nvim /etc/pam.d/system-login 
```
```
mkdir /etc/mkinitcpio.d
```
```
nvim /etc/cmdline.d/boot.conf 
```
```
nvim /etc/mkinitcpio.conf 
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

```
bootctl --path=/boot install 
```
```
mkinitcpio -P 
```
```
systemctl enable systemd-networkd 
```
```
systemctl enable iwd
```
```
systemctl enable firewalld 
```
```
systemctl enable sshd
```
```
reboot
```
