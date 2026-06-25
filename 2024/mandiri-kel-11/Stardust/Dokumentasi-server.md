## 1. BAGI PARTISI 
```
- lsblk
- pvcreate /dev/[nama partisi]
- vgcreate [nama grup] /dev/[nama partisi]
- lvcreate -L [size G | M] [nama grup] -n root
- lvcreate -L [size G | M] [nama grup] -n vars
- lvcreate -L [size G | M] [nama grup] -n vlog
- lvcreate -L [size G | M] [nama grup] -n vaud
- lvcreate -L [size G | M] [nama grup] -n vtmp
- lvcreate -L [size G | M] [nama grup] -n home
- lvcreate -l50%FREE [nama grup] -n podman
- lvcreate -l50%FREE [nama grup] -n [nama home untuk internal]
- lsblk
```
Luks on lvm home nya ada dua (eksternal dan internal) 

## 2. FORMAT PARTISI
```
- mkfs.ext4 /dev/[nama grup]/root
- mkfs.ext4 /dev/[nama grup]/vars
- mkfs.ext4 /dev/[nama grup]/vlog
- mkfs.ext4 /dev/[nama grup]/vaud
- mkfs.ext4 /dev/[nama grup]/vtmp
- mkfs.ext4 /dev/[nama grup]/home
- mkfs.ext4 /dev/[nama grup]/podman
```
kalo lupa partisi pake command lsblk -o name,fstype
```
mkfs.vfat -F32 /dev/[nama partisi boot]
```

## 3. MOUNTING
```
- mount /dev/[nama grup]/root /mnt
- mount —mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot /mnt/boot
- mount —mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
- mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
- mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
- mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
- mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/home /mnt/home
- mount —mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
(home untuk internal ga perlu dimounting, karena nanti mounting otomatis pake aplikasi pam_mount)
- cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
- YES
- bikin password
```
password nya harus sama kayak password user 


## 4. INSTALL PACKAGE
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
``` 
## 5. AFTER INSTALL
```
- genfstab -U /mnt > /mnt/etc/fstab
- echo “tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0” >> /mnt/etc/fstab
- cp /etc/systemd/network/* /mnt/etc/systemd/network
- arch-chroot /mnt
- echo [hostname] > /etc/hostname
- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
- hwclock —systohc
- nvim /etc/locale.gen (cari EN_US lalu hapus hashtag dua-duanya)
- locale-gen
- locale > /etc/locale.conf
- nvim /etc/locale.conf
- pacman -S pam_mount
- lsblk
```

## 6. SETUP HOME INTERNAL
```
- cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
- masukin password yg tadi udah dibuat
- mkfs.ext4 /dev/mapper/[device name]
- mkdir /home/[nama folder]
- useradd -d /home/[nama folder] [nama user]
- passwd [nama user]
- (masukin password. password harus sama kayak pas cryptsetup luksFormat)
- echo “[nama user] ALL‎ = (ALL:ALL)” > /etc/sudoers.d/[nama user]
- chown -R [nama user]:[nama user] /home/[nama folder]
- nvim /etc/security/pam_mount.conf.xml
- nvim /etc/pam.d/system-login
```

## 7. MKINITCPIO
```
- mkdir /etc/mkinitcpio.d
- nvim /etc/cmdline.d/boot.conf
- nvim /etc/mkinitcpio.conf
- nvim /etc/mkinitcpio.d/linux-hardened.preset
```

## 8. INSTALL SYSTEMD BOOT
```
- bootctl —path=/boot install
- mkinitcpio -P
- systemctl enable systemd-networkd
- systemctl enable systemd-networkd
- systemctl enable iwd
- systemctl enable firewalld
- systemctl enable sshd
- exit
```

Untuk Lenovo
```
bootctl —path=/mnt/boot install
```
```
reboot
```
