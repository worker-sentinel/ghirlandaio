# Install OS admin

## Cryptsetup dan format luks
```
sudo Cryptsetup luksFormat /dev/nvme0n1p5
ketik "YES"
```
masukin password
## Buka partisi luks
```
Cryptsetup luksOpen /dev/nvme0n1p5 novaria
```
Masukin passwd

```
pvcreate /dev/mapper/novaria
```
```
vgcreate kel4 /dev/mapper/novaria
```

```
lvcreate -L 8G kel4 -n root
mkfs.ext4 -b 4096 /dev/kel4/root
mount /dev/kel4/root /mnt
```
## BOOT
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p4
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p4 /mnt/boot
```
## apaya
```
lvcreate -L 8G kel4 -n vars
mkfs.ext4 -b 4096 /dev/kel4/vars
mount --mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vars /mnt/var
```

```
vcreate -L 1G kel4 -n vlog
mkfs,ext4 -b 4096 /dev/kel4/vtmp
mount –mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vtmp /mnt/var/tmp
```

```
lvcreate -L 1G kel4 -n vlog
mkfs.ext4 -b 4096 /dev/kel4/vlog
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log
```

```
lvcreate -L 1G kel4 -n vaud
mkfs.ext4 -b 4096 /dev/kel4/vaud
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log/audit
```

```
lvcreate -L 8G kel4 /dev/kel4/home
mkfs.ext4 -b 4096 /dev/kel4/home
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/home /mnt/home
```

```
lvcreate -L 5G kel4 -n podman
mkfs.ext4 /dev/kel4/podman
mount –mkdir -o rw,nosuid,relatime /dev/kel4/podman /mnt/var/lib/containers
```

```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman podman networkmanager
```

```
genfstab -U /mnt/ > /mnt/etc/fstab
cp /etc/system/network/* /mnt/etc/system/network
echo “tmpfs /tmp tmpfs deafults,nosuid,noexec,size=1G 0 0” >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

```
arch-root /mnt
```
## Timezone
```
ln -sf /esr/share/zoneinfo/Asia/Jakarta /etc/localtime’
hwclock --sytohc
```
```
nvim /etc/locale.gen
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1
```
>Hapus hastag-nya
```
en_US.UTF-8 UTF-8
en_US ISO-8859-1
esc kmudian :wq
```
## Localezation
```
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
paling atas LANG=en_US.UTF-8
paling bawah LC_ALL=en_US.UTF-8
:wq
```
## Tambah user
```
useradd -m Nayla
mskuin passwd
echo "nayla ALL=(ALL:ALL) ALL" > /etc/sudoers.d/nayla
```
```                                                                                                                                                                                                                                                                                                                                                                                   
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p5)=name novaria=/dev/kel4/root" >
/etc/cmdline.d/01-boot.conf
echo "rw" > /etc/cmdline.d/02-misc.conf 
```
```
nvim /etc/mkinitcpio.conf                                                                                                                                                                   
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck) kmudian :wq
```

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
ALL_config="/etc/mkinitcpio.conf"                                                                                                                                                                                                     
ALL_kver="/boot/kernel/vmlinuz-linux-lts"                                                                                                                                                                                                       
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"
 
#default_config="/etc/mkinitcpio.conf"                                                                                                                                                                                                          
#default_image="/boot/initramfs-linux-lts.img"                                                                                                                                                                                                  
default_uki="/boot/efi/Linux/arch-linux-lts.efi"                                                                                                                                                                                                
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"
```

```
bootctl --path=/boot install
pacman -S linux-lts linux-lts-headers
```

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
```

```
exit
umount -R /mnt
reboot
```




