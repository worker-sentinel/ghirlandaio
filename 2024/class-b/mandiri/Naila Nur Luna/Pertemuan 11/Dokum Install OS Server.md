# Panduan Instalasi OS Server 


---

1. Koneksi Jaringan (Wi-Fi)

Jika instalasi menggunakan jaringan Wi-Fi, gunakan utilitas `iwd`:

```
iwctl
# Di dalam prompt iwctl:
station wlan0 get-networks
station wlan0 connect "Imad"
# Masukkan password Wi-Fi Anda, lalu keluar
exit
```

2. Melihat Partisi Disk

```
lsblk
```

3. Membuat & Membuka Partisi LUKS
```
cryptsetup luksFormat /dev/nvme0n1p2
# Konfirmasi dengan mengetik YES, lalu masukkan password enkripsi
cryptsetup open /dev/nvme0n1p2 cryptroot
# Masukkan password yang sama seperti di atas
```

4. Setup LVM
```
pvcreate /dev/mapper/cryptroot
vgcreate kelzenith /dev/mapper/cryptroot
lvcreate -L 20G kelzenith -n root
lvcreate -L 10G kelzenith -n vars
lvcreate -L 5G kelzenith -n home
```

5. Format
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p1
mkfs.ext4 /dev/mapper/kelzenith-root
mkfs.ext4 /dev/mapper/kelzenith-home
mkfs.ext4 /dev/mapper/kelzenith-vars
```

6. Membuat direktori mount
```
mkdir -p /mnt/{boot,home,var,tmp}
```

7. Mounting
```
mount /dev/nvme0n1p1 /mnt/boot
mount /dev/mapper/kelzenith-root /mnt
mount /dev/mapper/kelzenith-home /mnt/home
mount /dev/mapper/kelzenith-vars /mnt/var
```

8. Install Package
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

9. Generate Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
10. Masuk arch root
```
arch-chroot /mnt

```
11. Mengatur zona waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

12. Locale
```
nvim /etc/locale.gen
# Uncomment baris: en_US.UTF-8 UTF-8
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
```
isi /etc/locale.conf jadi seperti ini:
```
LANG=en_US.UTF-8
LC_CTYPE="C.UTF-8"
LC_NUMERIC="C.UTF-8"
LC_TIME="C.UTF-8"
LC_COLLATE="C.UTF-8"
LC_MONETARY="C.UTF-8"
LC_MESSAGES=
LC_PAPER="C.UTF-8"
LC_NAME="C.UTF-8"
LC_ADDRESS="C.UTF-8"
LC_TELEPHONE="C.UTF-8"
LC_MEASUREMENT="C.UTF-8"
LC_IDENTIFICATION="C.UTF-8"
LC_ALL=
```

13. Membuat User
```
useradd -m kelompok10
passwd kelompok10
echo "kelompok10 ALL=(ALL:ALL) ALL" > /etc/sudoers.d/kelompok10
```

14. Mengatur Mkinitcpio
```
nvim /etc/mkinitcpio.conf
# Tambahkan sd-encrypt dan lvm2 sesudah "block":
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```
Edit preset untuk kernel linux-lts:
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
isi sesuai
```
ALL_config="/etc/mkinitcpio.d/default.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/boot/efi/linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"
```

15. Generate Initramfs
```
mkinitcpio -P
```

15. Install bootloader
```
bootctl --path=/boot install
ls -R /boot/loader
ls -lah /boot/EFI/Linux
```

16 Enable Layanan dasar
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable sddm
systemctl enable NetworkManager
```

17. Install desktop
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack network-manager-applet pipewire-alsa networkmanager firefox
```

18. Booting
```
exit
umount -R /mnt
reboot
```


