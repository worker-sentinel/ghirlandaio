# Dokumentasi Instalasi OS Client

## Menghubungkan internet

```
iwctl
device list
station wlan0 scan
station wlan0 get-network
station wlan0 connect “nama wifi”
exit
ping google.com
```

## Cek Partisi
```
lsblk
```

## Lalu bagi partisi
```
cfdisk /dev/sda
```

## Format partisi
```
mkfs.fat -F 32 /dev/sda5
mkswap /dev/sda7
swapon /dev/sda6
mkfs.ext /devsda7
mkfs.ext4 /dev/sda7
mount /dev/sda7 /mnt
```

```
mkfs.ext4 /dev/sda6
```

```
mkfs.fat -F 32 /dev/sda5
mount /dev/sda7 /mnt
mkdir -p /mnt/boot
mount /dev/sda5 /mnt/boot
```

## Install package
```
pacstrap -K /mnt base-devel linux linux-firmwarenvim intel-ucode
```

```
genfstab -U /mnt >> /mnt/etc/fstab
```

## Masuk sebagai root
```
arch-chroot /mnt
```

## Set Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
locale-gen
```

```
echo LANG=en_US.UTF-8 > /etc/locale.conf
cat /etc/locale.conf
echo comet > /etc/hostname
````
```
mkinitcpio -P
passwd
pacman -S grub efibootmgr networkmanager grub-install --target=x86_65-efi --efi-dictionary=/boot --bootloader-id=GRUB
```

## Install plasma
```
sudo pacman -S plasma kitty dolphin pipewire networkmanager sddm   
```

```
systemctl enable NetworkManager.service
systemctl enable sddm.service
```

```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

## Keluar
```
exit
umount -R /mnt

reboot
```
