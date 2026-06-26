# Dokumentasi instalasi os server app
```
```
## connect wifi
```
```
```
Iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect “nama wifi”
```
Masukan pw
```
Exit
```
```
Ping 8.8.8.8
```
```
```
```
Lsblk
```
```
```

## set up luks
```
Cryptsetup luksFormat /dev/sda6
```
```
Cryptsetup luksOpen /dev/sda6/way
```
```
Pvcreate /dev/mapper/way
```
```
Lsblk
```
```
Vgcreate lit /dev/mapper/way
```
```
Lsblk
```
```
Lvcreate -L 10G lit -n root
```
```
Lvcreate -L 10G lit -n vars
```
```
Lvcreate -L 1G lit -n vlog
```
```
Lvcreate -L 1G lit -n vaud
```
```
Lvcreate -L 1.5G lit -n vtmp
```
```
Lvcreate -l50%FREE lit -n home
```
```
Lvcreate -l00%FREE lit -n podman
```
```
Lsblk
```

## Formating LV
```
Mksfs.vfat -F32 -n BOOT /dev/nvme0n1p5
```
```
Mkfs.ext4 /dev/server/root
```
```
Mkfs.ext4 /dev/server/vars
```
```
Mkfs.ext4 /dev/server/vlog
```
```
Mkfs.ext4 /dev/server/vtmp
```
```
Mkfs.ext4 /dev/server/vaud
```
```
Mkfs.ext4 /dev/server/home
```
```
Mkfs.ext4 /dev/server/podman
```
```
Lsblk
```
```
```
## Mounting
```

Mount /dev/server/root /mnt
```
```
Mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p5 /mnt/boot
```
```
Mount –mkdir -o rw,nodev,nosuid,relatime /dev/server/vars /mnt/var
```
```
Mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vlog /mnt/var/log
```
```
Mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vaud /mnt/var/log/audit
```
```
Mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vtmp /mnt/var/tmp
```
```
Mount –mkdir -o rw,nodev,nosuid ,relatime /dev/server/home /mnt/home
```
```
Mount –mkdir -o rw,nodev,nosuid ,relatime /dev/server/podman /mnt/var/lib/containers
```
```
Lsblk
```
```
```
## Install Packages
```
Pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```

## Fstab
```

Genfstab -U /mnt > /mnt/etc/fstab
```
## Format tmpfs ke tmp
```

Echo “tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
``
```
## Chroot
```
Arch-chroot /mnt
```

## Membuat Hostname
```
Echo server > /etc/hostname
```

## Mengatur Waktu Dan Bahasa
```

Ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
Hwclock –systohc
```
```
Nvim /etc/locale.gen
```
```
>Hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
```
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
>Pada LANG=en_US.UTF-8 ubah menjadi LANG=en_US.UTF-8 kemudian dibagian LC_ALL= tambahkan juga en_US.UTF-8
```
## Menambahkan user
```
Useradd -m milky
```
```
Passwd milky
```
```
Masukkan pw
```

## Konfigurasi sudo
```
Echo “milky ALL=(ALL:ALL) ALL” > /etc/sudoers.d/milky
```
```
Su milky
```
```
Sudo su
```
```
Masukkan pw
```

## mkinitcpio
```
Mkdir /etc/cmdline.d
```
```
Touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
Lsblk
```
```
Echo “rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6=lit root=/dev/server/root)” > /etc/cmdline.d/01-boot.conf
```
```
Echo rw > /etc/cmdline.d/02-misc.conf
```
```
Nvim /etc/mkinitcpio.conf
```
```
>Pada hooks paling bawah, tambahkan sd-encrypt
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt  lvm2 block filesystems fsck)  
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>Hapus semua pagar pada baris ALL

>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus ‘efi’ dan ubah jadi ‘boot’
```
```
```
Bootctl –path=/boot install
```
```
Mkinitcpio -P 
```
```
Systemctl enable systemd-networkd
```
```
Systemctl enable systemd-resolved
```	
```
Systemctl enable iwd
```
```
Systemctl enable sshd
```
```
```

## Selesai
```
Exit
```
```
Umount -R /mnt
```
```
Reboot
```















