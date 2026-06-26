# TUTORIAL INSTALASI BASE ARCH LINUX
## 1. Download ISO Arch Linux
download iso arch linux menggunakan link berikut archlinux-2026.05.01-x86_64.iso  
## 2. Membuat Bootable USB

## 3. Boot ke flashdisk installer
colokkan flashdisk yang sudah berisi file bootable arch linux

restart laptop

tekan FN + F12


## 4. Sambungkan ke internet
gunakan ```iwctl```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 14 18" src="https://github.com/user-attachments/assets/91cdfef6-e15d-43a3-9300-e8bffa84a0dc" />


```station wlan0 scan```

```station wlan0 get-networks```

```station wlan0 connect namawifi```
masukkan password wifi jika diminta

## 5. Cek struktur disk dan partisi
```lsblk```

untuk melihat nama disk, ukuran partisi, struktur storage

contoh disk ```/dev/nvme0n1```

## 6. Membuat Partisi
masuk ke cfdisk
```cfdisk /dev/nvme0n1```

## 7. Membagi Partisi
Pilih Free Space

<img width="1600" height="901" alt="WhatsApp Image 2026-05-20 at 00 54 32" src="https://github.com/user-attachments/assets/edf9a39c-371c-4745-8905-bef470d9e60b" />

Lalu bagi partisi sesuai pengkondisian penyimpanan yang sudah dibagi, kelompok kami memakai

1 GB untuk boot, type EFI system

4 GB untuk swap, type Linux swap

14 GB untuk root, type Linux filesystem

## 8. Membuat filesystem
```lsblk```

contoh hasilnya 

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 20 41" src="https://github.com/user-attachments/assets/4cedffd5-60b1-44dd-86d3-a1760ae41922" />


/dev/nvme0n1p5 untuk boot

/dev/nvme0n1p6 untuk swap

/dev/nvme0n1p7 untuk root

format root ke ext4

```mkfs.ext4 /dev/nvme0n1p7```

format partisi tambahan

```mkfs.ext4 /dev/nvme0n1p6```

aktifkan swap (swap digunakan sebagai cadangan ketika RAM penuh)

```mkswap /dev/nvme0n1p6```

```swapon /dev/nvme0n1p6```

format boot ```mkfs.fat -F 32 /dev/nvme0n1p5```

## 9. Mount Partisi

Partisi harus dipasang sebelum sistem diinstall

mount root ```mount /dev/nvme0n1p7 /mnt```

membuat folder boot ```mkdir -p /mnt/boot```

mount boot ```mount /dev/nvme0n1p5 /mnt/boot```

## 10. Install base system
install package dasar arch linux ```pacstrap -K /mnt base-devel linux linux-firmwarenvim amd-ucode (jika intel maka menjadi intel-ucode)```

## 11. Generate fstab 
fstab digunakan untuk menyimpan konfigurasi mount partisi 

```genfstab -U /mnt >> /mnt/etc/fstab```

## 12. Masuk ke  sistem arch
```arch-chroot /mnt```

## 13. Mengatur timezone
contoh mengatur sesuai timezone Indonesia

```In -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```

lalu sinkronkan dengan hardware clock ```hwclock --systoch```


Setelah file Arch Iso berhasil diunduh, langkah berikutnya adalah mengunduh __Rufus__. ketik __[rufus.ie/id/](https://rufus.ie/id/)__ untuk mengakses situs Rufus berbahasa indonesia, lalu klik unduh untuk melakaukan pengunduhan.
## 3. Boot ke flashdisk installer
Colokkan flashdisk yang sudah berisi file bootable arch linux

Restart laptop

tekan Fn + F12 (bisa juga F2, Del, atau Esc)


## 4. Sambungkan ke internet
gunakan ```iwctl```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 14 18" src="https://github.com/user-attachments/assets/91cdfef6-e15d-43a3-9300-e8bffa84a0dc" />


```station wlan0 scan```

```station wlan0 get-networks```

```station wlan0 connect namawifi```
masukkan password wifi jika diminta

## 5. Cek struktur disk dan partisi
```lsblk```

untuk melihat nama disk, ukuran partisi, struktur storage

contoh disk ```/dev/nvme0n1```

## 6. Membuat Partisi
masuk ke cfdisk
```cfdisk /dev/nvme0n1```

## 7. Membagi Partisi
Pilih Free Space

<img width="1600" height="901" alt="WhatsApp Image 2026-05-20 at 00 54 32" src="https://github.com/user-attachments/assets/edf9a39c-371c-4745-8905-bef470d9e60b" />

Lalu bagi partisi sesuai pengkondisian penyimpanan yang sudah dibagi, kelompok kami memakai

<img width="900" height="1600" alt="WhatsApp Image 2026-05-20 at 02 58 09" src="https://github.com/user-attachments/assets/55c4ebbf-ed78-4e9e-b95b-aba03ffc0b13" />

1 GB untuk boot, type EFI system

4 GB untuk swap, type Linux swap

14 GB untuk root, type Linux filesystem

## 8. Membuat filesystem
```lsblk```

contoh hasilnya 

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 20 41" src="https://github.com/user-attachments/assets/4cedffd5-60b1-44dd-86d3-a1760ae41922" />


/dev/nvme0n1p5 untuk boot

/dev/nvme0n1p6 untuk swap

/dev/nvme0n1p7 untuk root

format root ke ext4

```mkfs.ext4 /dev/nvme0n1p7```

format partisi tambahan

```mkfs.ext4 /dev/nvme0n1p6```

aktifkan swap (swap digunakan sebagai cadangan ketika RAM penuh)

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 18 54" src="https://github.com/user-attachments/assets/9088e8bf-280f-4aba-8194-ae34fee01dd3" />

```mkswap /dev/nvme0n1p6```

```swapon /dev/nvme0n1p6```

format boot ```mkfs.fat -F 32 /dev/nvme0n1p5```

## 9. Mount Partisi

Partisi harus dipasang sebelum sistem diinstall

<img width="1280" height="720" alt="WhatsApp Image 2026-05-20 at 02 17 32" src="https://github.com/user-attachments/assets/b5720d7f-0751-4d35-b2f9-8add2ac5b95b" />


mount root ```mount /dev/nvme0n1p7 /mnt```

membuat folder boot ```mkdir -p /mnt/boot```

mount boot ```mount /dev/nvme0n1p5 /mnt/boot```

## 10. Install base system
install package dasar arch linux ```pacstrap -K /mnt base-devel linux linux-firmwarenvim amd-ucode (jika intel maka menjadi intel-ucode)```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 20 41" src="https://github.com/user-attachments/assets/9d9af8df-73c9-4012-af1c-acc51acf7089" />


## 11. Generate fstab 
fstab digunakan untuk menyimpan konfigurasi mount partisi 

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 12 18" src="https://github.com/user-attachments/assets/a9a60c09-749c-4311-827b-b19cb1385c7a" />


```genfstab -U /mnt >> /mnt/etc/fstab```

## 12. Masuk ke  sistem arch
```arch-chroot /mnt```

## 13. Mengatur timezone
contoh mengatur sesuai timezone Indonesia

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 12 18 (1)" src="https://github.com/user-attachments/assets/5297bdf5-33da-41c5-934f-aa9bb6119a8d" />

```In -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```

lalu sinkronkan dengan hardware clock ```hwclock --systoch```

## 14. Localization
Generate locale
```locale-gen```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 12 19 (1)" src="https://github.com/user-attachments/assets/c96a921a-d6c7-4b5e-9b1a-8e1b2fbbf9be" />

isi ```/etc/locale/conf```:
```LANG=en_US.UTF-8```
Untuk mengecek bisa ketik
```cat /etc/locale.conf```

## 15. Hostname
Buat nama komputer di Jaringan
```echo > /etc/hostname```
contoh
<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 58 10" src="https://github.com/user-attachments/assets/6a5d8257-d1b7-44ef-a3fd-6fc6b6290653" />

```echo fira > /etc/hostname```
Setelah itu ```mkinitcpio -P```

## 16. Passwod Root
Buat Password untuk administrator/root
```passwd```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 12 21" src="https://github.com/user-attachments/assets/b69c6322-b07a-4c03-8990-f3efdef976f3" />

## 17. Install Bootloader
Install bootloader + network
```pacman -S grub efibootmgr networkmanager```
```grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB```
lanjut ke ```grub-mkconfig -o /boot/grub/grub.cfg```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 02 12 21" src="https://github.com/user-attachments/assets/d26e9df2-c7ba-4994-9510-ef0fadb6b4a1" />

## Reboot
Keluar dari chroot
```exit```
Umount
```umount -R /mnt```
Reboot
```reboot```
Lepas USB installer setelah restart
