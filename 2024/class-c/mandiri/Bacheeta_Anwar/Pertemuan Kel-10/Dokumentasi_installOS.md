#### Install OS Server
##### Hubungkan ke internet dul

```‎
iwctl
```
‎
**aktifkan asciinema**

‎```
lsblk
‎```

**cryptsetup dan format luks**

```
cryptsetup luksFormat /dev/partisi sistem
```
```
cryptsetup luksOpen /dev/partisi sistem proc
```
```
pvcreate /dev/mapper/proc
```
```
vgcreate [nama volume grup] /dev/mapper/proc
```

**cek partisi**

```
lsblk
```
‎
**Buat logical volume**
```
lvcreate -L 8G comet -n root
```
```
lvcreate -L 5G comet -n vars
```
```
lvcreate -L 1G comet -n vlog
```
```
lvcreate -L 1G comet -n vaud
```
```
lvcreate -L 1.5G comet -n vtmp
```
```
lvcreate -l50%FREE comet -n home
```
```
lvcreate -l100%FREE comet -n podman
```
**Cek Partisi**
```
lsblk
```

**Format partisi**

```
mkfs.ext4 /dev/comet/root
```
```
mkfs.vfat -F32 /dev/partisi boot
```
```
mkfs.ext4 /dev/comet/vars
```
```
mkfs.ext4 /dev/comet/vlog
```
```
mkfs.ext4 /dev/comet/vaud
```
```
mkfs.ext4 /dev/comet/vtmp
```
```
mkfs.ext4 /dev/comet/home
```
```
mkfs.ext4 /dev/comet/podman
```
**Mount**

```
mount /dev/comet/root /mnt
```
**Cek kembeli**
```
lsblk
```
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi boot /mnt/boot
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/vars /mnt/var
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vlog /mnt/var/log
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vaud /mnt/var/log/audit
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vtmp /mnt/var/tmp
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/home /mnt/home
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/podman /mnt/var/lib/containers
```
**Cek lagi**

```
lsblk
```
**Install Package**

```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
**Fstab**
```
genfstab -U /mnt/etc/fstab
```
```
echo “tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
**Menyalin settingan jaringan**

```
cp /etc/systemd/network/*  /mnt/etc/systemd/network
```
**Masuk ke sistem**
```
arch-chroot /mnt
```
**Hostname**
```
echo server > /etc/hostname
```
**Sinkronasi waktu**
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock –systohc
```
**Mengatur bahasa**

```
nvim /etc/locale.gen
```
**encommanting en_US**
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
**Ubah**
LANG=C.UTF-8 **(LANG=en_US.UTF-8)**
LC_ALL= **(ditambahin en_US.UTF-8)**

**Buat user**
```
useradd -m catchers
```
```
passwd catchers
```
```
echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/catchers
```
```
su catchers
```
```
sudo su
```

**enter password** 

**Mengatur parameter**
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
**cek kembali**
```
lsblk
```
**Parameter**
```
cat /etc/cmdline.d/01-boot.conf
```
```
echo “rd.luks.name=$(blkid -s UUID -o value /dev/partisi boot)=proc root=/dev/comet/root” > /etc/cmdline.d/01-boot.conf
```
```
echo rw > /etc/cmdline.d/02-misc.conf
```
```
nvim /etc/mkinitcpio.conf
```

bagian HOOKS akhir tambahin **(sd-encrypt lvm2)**

**Mengatur mkinitcpio**
```
nvim /etc/mkinitcpio.d/linux-lts.conf.preset
```
```
#mkinitcpio preset file for the 'linux-lts' package
ALL_config="/etc/mkinitepio.conf"
ALL_kver="/boot/vmlinuz-linux-Its"
ALL_kerneldest="/boot/vmlinuz-linux-lts"


PRESETS=( 'default')
#PRESETS=('default' ‘fallback')

#default_config="/etc/mkinitcpio.conf”
#default_image="/boot/initramfs-linux-lts.img”
 default_uki="/boot/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /us/share/systend/bootctl/splash-arch.bmp”
#fallback_config=”/etc/mkinitcpio.conf”
#fallback_image="/boot/initramfs-linux-lts-fallback.img “
#fallback_uki=”/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect”
```

**Install bootctl**
```
bootctl –path=/boot install
```
```
mkinitcpio -P
```
**Aktifkan**
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
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

exit
umount -R /mnt

reboot












‎

