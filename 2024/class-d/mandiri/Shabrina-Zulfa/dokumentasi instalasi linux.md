# Cara Install Arch Linux

## Nama: Shabrina Zulfa 
## Kelas: 4D  
## NIM: 12402051010024  
## Mata Kuliah: Perpustakaan dan Arsip Digital  
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum  

---

# 1. Persiapan

- Masukkan flashdisk ke laptop atau komputer.
- Cari dan download Rufus versi standard.
- Download Arch Linux ISO versi Indonesia ukuran sekitar 1GB.
<img width="1040" height="585" alt="WhatsApp Image 2026-05-23 at 20 10 38 (1)" src="https://github.com/user-attachments/assets/de3d5545-a5ef-4271-84e8-1f87506fb526" />
<img width="1040" height="585" alt="WhatsApp Image 2026-05-23 at 20 10 39" src="https://github.com/user-attachments/assets/fe493a30-8713-433e-a8a1-4883ce957914" />

---

# 2. Membuat Bootable USB

- Buka Rufus.
- Pada bagian Device pilih nama flashdisk masing-masing.
- Klik Select lalu pilih file Arch Linux ISO yang sudah didownload.
- Klik Open.
- Klik Start.
- Pilih opsi 2 lalu klik OK.
- Tunggu proses selesai.
- Klik Close.
<img width="1040" height="585" alt="WhatsApp Image 2026-05-23 at 20 10 39 (1)" src="https://github.com/user-attachments/assets/ba3c01ed-1258-4544-a42b-305704850846" />

---

# 3. Booting ke Arch Linux

- Restart laptop.
- Tekan `F2` untuk masuk BIOS.
- Masuk ke menu:
  - Security
  - Secure Boot
  - Disabled
- Tekan `F10` untuk save lalu exit.
- Pilih boot menggunakan flashdisk.
<img width="1040" height="585" alt="WhatsApp Image 2026-05-23 at 20 10 39 (2)" src="https://github.com/user-attachments/assets/fa59ab2d-d6dd-4a8a-915e-ad130703eb73" />
<img width="1040" height="585" alt="WhatsApp Image 2026-05-23 at 20 10 40" src="https://github.com/user-attachments/assets/e891590f-d37c-493e-83c3-2e5cccb18a4e" />

---

# 4. Mengecek Mode EFI

Ketik:

```bash
cat /sys/firmware/efi/fw_platform_size
```

---

# 5. Menghubungkan ke Internet

Masuk ke iwctl:
iwctl
<img width="1040" height="585" alt="WhatsApp Image 2026-05-24 at 00 06 39" src="https://github.com/user-attachments/assets/59949366-a317-4edc-b5e6-54fb132b15df" />
<img width="1040" height="585" alt="WhatsApp Image 2026-05-24 at 00 06 39 (3)" src="https://github.com/user-attachments/assets/207f0ddc-3877-4147-947b-24f4c1a5b90b" />

```

Cek driver Wi-Fi:
device list
<img width="1040" height="585" alt="WhatsApp Image 2026-05-24 at 01 12 24" src="https://github.com/user-attachments/assets/9d38897b-1679-4117-a121-b304de7d414b" />

```

Scan Wi-Fi:
station wlan0 scan
<img width="1040" height="585" alt="WhatsApp Image 2026-05-24 at 01 14 33" src="https://github.com/user-attachments/assets/d267b954-ea79-4856-ad1f-d949ffa1a500" />

```

Melihat daftar Wi-Fi:
station wlan0 get-networks
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/7703bfef-0dd0-4a0c-88f0-0719a5fb70f1" />
```

Menghubungkan Wi-Fi:
station wlan0 connect nama-wifi
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/e76d33b3-6af8-4ebe-912b-9d4148c6c3c5" />

```

Masukkan password Wi-Fi.

Keluar:
exit
```

Cek koneksi internet:

```bash
ping 1.1.1.1
```

---

# Perkondisian

Mengaktifkan Wi-Fi:

```bash
device wlan0 set-property Powered on
```

Jika tidak bisa:

```bash
rfkill list
rfkill unblock all
```

Untuk hidden network:

```bash
station wlan0 connect-hidden rogerlab
```

Masukkan password:

```text
T1g4_Puluh_R1bu
```

---

# 6. Mengecek Partisi

Melihat partisi:
lsblk -o name,fstype,size
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/7c8b57cb-2cbc-4b8f-b46e-2bee575c62ed" />
```

Keterangan:
- `vfat` digunakan untuk sistem UEFI.
- `ntfs` digunakan untuk Windows.

---

# 7. Membagi Partisi

Masuk ke cfdisk:

```bash
cfdisk /dev/nvme0n1
```

Membuat partisi:
- Boot = `1G` → EFI System
- Swap = `4G` → Linux Swap
- Root = `14.2G` → Linux Filesystem

Contoh:
- Boot = `/dev/nvme0n1p5`
- Swap = `/dev/nvme0n1p6`
- Root = `/dev/nvme0n1p7`

---

# 8. Format Partisi

Format root:

```bash
mkfs.ext4 /dev/nvme0n1p7
<img width="1040" height="585" alt="WhatsApp Image 2026-05-24 at 07 12 44" src="https://github.com/user-attachments/assets/78277c6d-1878-44c3-b231-3f4b8a23c38d" />
```

Format swap:
mkswap /dev/nvme0n1p6
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/82611a21-5b50-46ca-af74-dc885f875438" />
```

Aktifkan swap:
swapon /dev/nvme0n1p6
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/a9177abd-c6bc-47b1-833d-b0bfbef73c40" />
```

Format EFI:
mkfs.fat -F32 /dev/nvme0n1p5
<img width="4160" height="2340" alt="image" src="https://github.com/user-attachments/assets/d61baa0b-9c04-4f4f-b7f7-e0bb1260f6d2" />
```

---

# 9. Mount Partisi

Mount root:
mount /dev/nvme0n1p7 /mnt
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/19bb6288-6b11-4f74-a83b-bc0b097bd25a" />
```

Mount EFI:

```bash
mount --mkdir /dev/nvme0n1p5 /mnt/boot
<img width="1280" height="410" alt="image" src="https://github.com/user-attachments/assets/d59426c6-ff7e-4d1d-9160-e2df9ad4e22b" />
```

---

# 10. Instalasi Sistem Dasar

Install sistem dasar Arch Linux:

```bash
pacstrap -K /mnt base base-devel linux linux-firmware neovim git networkmanager
```

---

# 11. Membuat FSTAB

FSTAB digunakan untuk menentukan partisi yang otomatis dimount saat boot.

genfstab -U /mnt >> /mnt/etc/fstab
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/a9def841-9e74-4e8b-be72-b49f01f2f869" />
```

Masuk ke sistem Arch:

arch-chroot /mnt
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/3fc94649-14d7-4e63-a880-d0b6b4d20403" />
```

Digunakan untuk mengubah root install menjadi sistem Arch Linux yang baru dipasang.

---

# 12. Download Pengelola Jaringan

Install iwd dan dhcpcd:

```bash
pacman -S iwd dhcpcd
```

Ketik:

```text
Y
```

---

# 13. Mengaktifkan Service

```bash
systemctl enable iwd.service
systemctl enable dhcpcd.service
```

---

# 14. Mengatur Timezone

Sinkronasi waktu Indonesia:

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Sinkronkan hardware clock:

```bash
hwclock --systohc
```

---

# 15. Localization

Cek neovim:

```bash
nvim
```

Cara keluar:

```text
:q
```

Generate locale:

```bash
locale-gen
```

Isi locale.conf:

```bash
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

Cek file:

```bash
cat /etc/locale.conf
```

---

# 16. Membuat Hostname

```bash
echo "bina" > /etc/hostname
```

Hostname digunakan sebagai nama komputer.

---

# 17. Generate Initramfs

Membuat image boot awal Linux:

```bash
mkinitcpio -P
```

---

# 18. Membuat Password Root

```bash
passwd
```

Masukkan password root.

---

# 19. Menambahkan User

Membuat user baru:

```bash
useradd -m shabrina
```

Membuat password user:

```bash
passwd shabrina
```

Menambahkan sudo:

```bash
echo "shabrina ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/none
```

Digunakan untuk memberikan akses sudo pada user.

---

# 20. Install Bootloader

Install GRUB:

```bash
pacman -S grub efibootmgr
```

Install bootloader:

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

Generate konfigurasi GRUB:

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

# 21. Reboot

Keluar dari chroot:

```bash
exit
```

Unmount partisi:

```bash
umount -R /mnt
```

Restart:

```bash
reboot
```

Setelah laptop mati, cabut flashdisk.

---

# 22. Menyalakan Service iwd dan dhcpcd

Menyalakan service:

```bash
systemctl start iwd.service
systemctl start dhcpcd.service
```

Cek status service:

```bash
systemctl status iwd.service
systemctl status dhcpcd.service
```

---

# Selesai

Arch Linux berhasil diinstall.
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/01004ffc-dc88-4233-b51f-fc1720b95e48" />
<img width="1040" height="585" alt="image" src="https://github.com/user-attachments/assets/b8f487eb-8927-4633-9ef8-3f531d054aad" />
