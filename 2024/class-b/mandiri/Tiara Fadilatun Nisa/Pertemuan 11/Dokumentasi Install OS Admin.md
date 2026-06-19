# Instalasi OS Admin

**1. Menyambungkan Wifi**
```
iwctl
station wlan0 get-networks
station wlan0 connect “Imad” (nama wifi)
Selanjutnya masukkan password
exit 
```


**2. Melihat Partisi DISK**
```
lsblk
```


**3. Pembuatan Enkripsi LUKS**
```
cryptsetup luksFormat /dev/nvme0n1p6
```


**4. Membuka Enkripsi**
```
cryptsetup luksOpen /dev/nvme0n1p6 matcha 
```


**5. Membuat Physical Volume (PV)**
```
pvcreate /dev/mapper/matcha
```


**6. Membuat Volume Group**
```
vgcreate latte /dev/mapper/matcha
```


**7. Membuat Logical Volume**
```
lvcreate -L 10G latte -n root
lvcreate -L 10G latte -n vars
lvcreate -L 1G latte -n vlog
lvcreate -L 1G latte -n vtmp
lvcreate -L 1G latte -n vaud
lvcreate -L 10G latte -n home
lvcreate -L 10G latte -n podman
```


**8. Verifikasi Hasil** 
```
lsblk
```


**9. Membuat Filesystem**

***EFI Partition***
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p5
```

***Root***
```
mkfs.ext4 /dev/latte/root
vars
vtmp
vaud
vlog
home
podman
```


**10. Mount Partisi**

***Boot***
```
mount --mkdir \ -o uid=0,gid=0,fmask=0077,dmask=0077 \
/dev/nvme0n1p5 /mnt/boot
```

***Root***
```
  mount /dev/latte/root /mnt
	mount --mkdir -o rw,nodev,nosuid,relatime  /dev/latte/vars /mnt/var
	mount --mkdir -o rw,nodev,nosuid,noexec,relatime  /dev/latte/vtmp /mnt/var/tmp
	mount --mkdir -o rw,nodev,nosuid,noexec,relatime  /dev/latte/vlog /mnt/var/log
	mount --mkdir -o rw,nosuid,noexec,relatime  /dev/latte/vaud /mnt/var/log/audit
  mount --mkdir -o rw,nosuid,noexec,relatime  /dev/latte/home /mnt/home
	mount --mkdir -o rw,nosuid,noexec,relatime  /dev/latte/podman /mnt/podman
```


**11. Instalasi Sistem Dasar Arch Linux**
```
  pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```


**12. Pembuatan File fstab**
```
genfstab -U /mnt > /mnt/etc/fstab
```


**13. Menyalin Konfigurasi Network**
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```


**14. Menambahkan tmpfs ke fstab**
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```


**15. Isi fstab**
```
cat /mnt/etc/fstab
```


**16. Masuk ke Sistem yang Baru** 
```
arch-chroot /mnt
```


**17. Konfigurasi Zona Waktu** 
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```


**18. Sinkronisasi Hardware Clock**
```
hwclock --systohc
```


**19. Konfigurasi Locale Sistem**
```
nvim /etc/locale.gen 
```

  
**20. Menyimpan Konfigurasi Locale**
```
locale > /etc/locale.conf
```


**21. Mengedit File Locale**
```
nvim /etc/locale.conf
```


**22. Membuat User Baru**
```	
useradd -m zenith
```


**23. Memberikan Password**
```
passwd zenith
```


**24. Memberikan Hak Sudo**
```	
echo "zenith ALL=(ALL:ALL) ALL" > /etc/sudoers.d/zenith
```


**25. Membuat Direktori Kernel Command Line**
```	
mkdir /etc/cmdline.d
```


**26. Membuat File Konfigurasi**
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```


**27. Konfigurasi LUKS dan Root Filesystem**
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6)=matcha root=/dev/latte/root" > /etc/cmdline.d/01-boot.conf
```


**28. Menambahkan Parameter rw**
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```


**29. Konfigurasi mkinitcpio** 
```
nvim /etc/mkinitcpio.conf
```


**30. Konfigurasi Proses Boot Kernel Linux-LTS**
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```


**31. Konfigurasi Virtual Console**
```
touch /etc/vconsole.conf
```


**32. Instalasi Bootloader Systemd-boot**
```
bootctl --path=/boot install
```


**33. Mengecek Isi Folder Boot**
```
  cd /boot
  ls
```


**34. Membuat Initramfs**
```
mkinitcpio -P
```


**35. Konfigurasi Preset Kernel Linux-LTS**
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```


**36. Instalasi Graphical User Interface (GUI)**
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack network-manager-applet pipewire-alsa networkmanager firefox
```


**37. Konfigurasi dan Aktivasi Firewall**
```
systemctl enable firewalld
```


**38. Konfigurasi Layanan DNS Sistem**
```
systemctl enable systemd-resolved
```


**39. Konfigurasi Layanan Container**
```
systemctl enable podman.socket
```

**40. Booting**
```
exit
umount -R /mnt
reboot
```






