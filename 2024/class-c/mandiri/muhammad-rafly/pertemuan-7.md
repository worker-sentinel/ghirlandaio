# Instalasi Arch Linux
## Preparation
- Siapkan bootable USB yang berisi ISO Arch Linux.
- Masuk ke mode BIOS dan nonaktifkan secure boot.
  ![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.04.jpeg)
- Lakukan boot ke USB dan reboot.
   ![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.03.jpeg)

## Live Environment
Pilih yang pailing atas `Arch Linux install medium (x86_64, UEFI)`

## Menghubungkan internet
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.03%20(2).jpeg)
```
iwctl
```
Cek wifi
```
device list
```
menyambungkan koneksi wifi
```
station wlan0 connect "nama wifi"
```
tes koneksi
```
ping 8.8.8.8
```

## Sinkronisasi waktu
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.02.jpeg)
```
timedatectl
```

## Partisi Disk
lakukan cek partisi 
```
lsblk
```
buat partisi
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01.jpeg)
```
cfdsik /dev/the_disk_to_be_partitioned
```
buat partisi `EFI`, `swap`, `root`
cek kembali parisi yang telah dibuat dengan `lsblk`

## Format partisi
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.02%20(1).jpeg)
format partisi root
```
mkfs.ext4 /dev/root_partition
```
format partisi swap
```
mkswap /dev/nvme0n1p2
```
aktifkan swap
```
swapon /dev/swap_partition
```
format partisi EFI
```
mkfs.fat -F 32 /dev/efi_system_partition
```

## Mount filesystem
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01%20(1).jpeg)
mount root
```
mount /dev/root_partition /mnt
```
mount EFI
```
mount --mkdir /dev/efi_system_partition /mnt/boot
```
cek kembali partisi yang telah di mounting dengan `lsblk`
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01%20(2).jpeg)

## Instalasi sistem
```
pacstrap -K /mnt base linux linux-firmware base base-devel networkmanager
```

## Membuat fstab, masuk ke arch-chroot, dan mengatur timezone
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.00.jpeg)
membuat fstab 
```
genfstab -U /mnt >> /mnt/etc/fstab
```
masuk ke arch-chroot
```
arch-chroot /mnt
```
mengatur timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
sinkronkan hardware clock
```
hwclock --systohc
```

## Localization
Sebelumnya instal dulu teks editor seperti neovim
```
pacman -S neovim
```
uncomenting en_US di /etc/locale.gen
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.00%20(1).jpeg)
```
nvim /etc/locale.gen
``` 
generate locale
```
locale-gen
```
```
locale > /etc/locale.conf
```
config locale
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.59.jpeg)
```
nvim /etc/locale.conf
```
Isi `LANG=` dan `LC_ALL=` dengan `en_US.UTF-8`

## Generate Initramfs, membuat hostname, password, dan user
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.59%20(1).jpeg)
```
mkinitcpio -P
```
isi file /etc/hostname dengan nama hostname yang ingin dibuat
```
nvim /etc/hostname
```
buat password root
```
passwd
```
buat user dan password user
```
useradd -m -G wheel -s /bin/bash "nama user"
```
```
passwd user
```

## Install bootloader GRUB
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.58.jpeg)
```
pacman -S grub efibootmgr
```
instalasi bootloader ke partisi EFI
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB2
```
membuat file konfigurasi
```
grub-mkconfig -o /boot/grub/grub.cfg
```
Install os prober untuk melakukan dual boot di GRUB
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.58%20(1).jpeg)
```
pacman -S os-prober
```
aktifkan os prober
```
echo "GRUB_DISABLE_OS_PROBER=false" >> /etc/default/grub
```
update kembali
```
grub-mkconfig -o /boot/grub/grub.cfg
```

## Reboot
keluar dari arch-chroot
```
exit
```
umount
```
umount -R /mnt
```
reboot
```
reboot
```
lepas usb ketika layar sudah mati.

---

# Instalasi Desktop Environment KDE Plasma
## Memberikan hak akses sudo ke user
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.56.jpeg)
```
sudo EDITOR=nvim visudo
```
uncometing `# %wheel ALL=(ALL:ALL) ALL`

## Aktifkan network manager dan sambungkan koneksi internet
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.56%20(1).jpeg)
```
systemctl enable --now NetworkManager
```
sambungkan koneksi internet
```
nmtui
```
pilih `Activate a connection`

## Install plasma
![alt text](https://github.com/Rafly-87/Studying/blob/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.55.jpeg)
```
sudo pacman -S plasma sddm pipewire dolphin kitty
```
masuk ke KDE plasma
```
sudo systemctl enable --now sddm
```
Akan mucul tampilan seperti ini dan masukkan password
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.55%20(1).jpeg)

Tampilan filemanager dolphin
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54.jpeg)

Tampilan network Network Manager
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54%20(2).jpeg)

Tampilan audio pipewire
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54%20(1).jpeg)
