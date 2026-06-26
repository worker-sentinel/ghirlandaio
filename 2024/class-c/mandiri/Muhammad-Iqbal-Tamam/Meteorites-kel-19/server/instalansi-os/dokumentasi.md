## partisi
```
cfdisk /dev/partisi
```
### pembagian partisi
```
boot = 5G efi system
system = sisanya linux filesystem
```

## setup luks
```
cryptsetup luksFormat --sector-size 4096 /dev/partisisystem
cryptsetup luksOpen /dev/partisi_system oxva
pvcreate /dev/mapper/oxva
vgcreate xlim /dev/mapper/oxva
```
## setup LVM
### root
### boot
### vars
### tmp
### vtemp
### vlog
### vaud
### dock
### home

## install package
```
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema linux-firmware-realtek firewalld
```

## registrasi partisi
```
genfstab -U /mnt > /mnt/etc/fstab
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

## chroot
```
arch-chroot /mnt
```

## membuat hostname 
```
echo iqbal > /etc/hostname
```

## time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
nvim /etc/locale.gen
```
uncommenting en_US
```
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
```

## user
```
useradd -m iqbal
passwd iqbal
echo "iqbal ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/iqbal
```

## kernel parameter
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi_luks])=xlim root=/dev/oxva/root" > /etc/cmdline.d/01-boot.conf
echo "rw" > /etc/cmdline.d/06-misc.conf
```

## mkinitcpio
```
cd /boot
mkdir kernel efi
cd efi
mkdir linux
cd ..
mv vmlinuz-ltsl-preset intel-lts kernel
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
di sesuaikan
```
bootctl --path=/boot install
mkinitcpio -P
systemctl enable iwd
systemctl enable systemd-networkd.socket
systemctl enable systemd-resolved
```
## selesai
```
exit
umount -R /mnt
reboot
```
