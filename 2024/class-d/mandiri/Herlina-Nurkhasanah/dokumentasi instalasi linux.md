## Nama: Herlina Nurkhasanah
## NIM: 12402051030101
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum

## Konteksi
instalasi linux dari kelompok 5 & 6

# 1. Persiapan

- Masukkan flashdisk ke laptop atau komputer.
- Cari dan download Rufus versi standard.
- Download Arch Linux ISO versi Indonesia ukuran sekitar 1GB.

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

---

# 4. Mengecek Mode EFI

Ketik:

```bash
cat /sys/firmware/efi/fw_platform_size
```

---

# 5. Menghubungkan ke Internet

Masuk ke iwctl:

```bash
iwctl
```

Cek driver Wi-Fi:

```bash
device list
```

Scan Wi-Fi:

```bash
station wlan0 scan
```

Melihat daftar Wi-Fi:

```bash
station wlan0 get-networks
```

Menghubungkan Wi-Fi:

```bash
station wlan0 connect nama-wifi
```

Masukkan password Wi-Fi.

Keluar:

```bash
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

```bash
lsblk -o name,fstype,size
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
```

Format swap:

```bash
mkswap /dev/nvme0n1p6
```

Aktifkan swap:

```bash
swapon /dev/nvme0n1p6
```

Format EFI:

```bash
mkfs.fat -F32 /dev/nvme0n1p5
```

---

# 9. Mount Partisi

Mount root:

```bash
mount /dev/nvme0n1p7 /mnt
```

Mount EFI:

```bash
mount --mkdir /dev/nvme0n1p5 /mnt/boot
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

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Masuk ke sistem Arch:

```bash
arch-chroot /mnt
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
echo "Nova" > /etc/hostname
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
useradd -m nuraini
```

Membuat password user:

```bash
passwd nuraini
```

Menambahkan sudo:

```bash
echo "nuraini ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/none
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

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 16 56 59" src="https://github.com/user-attachments/assets/71970484-45f2-4d30-9ebd-7c21d776e8cd" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 09 53" src="https://github.com/user-attachments/assets/8ec27863-ce97-49fc-b3c2-9b769477f7e4" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 11 23" src="https://github.com/user-attachments/assets/4c7aabe5-2f4f-4756-88e8-6dc1edcfebfc" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 13 42" src="https://github.com/user-attachments/assets/30c32623-8786-4770-957d-6fa10ff3d16b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 14 54" src="https://github.com/user-attachments/assets/6c075442-01e4-438d-82ba-acaf606c569b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 16 07" src="https://github.com/user-attachments/assets/3ef86b1a-2540-490e-830d-fc26290b0f2b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 16 40" src="https://github.com/user-attachments/assets/8e4cce13-481b-4784-8d3f-eac0f408dc28" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 17 22" src="https://github.com/user-attachments/assets/7d9ec707-055a-4058-894d-53fb61b84ecc" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 17 58" src="https://github.com/user-attachments/assets/b4373a43-3ceb-4646-af8c-64dfb981c03c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 18 46" src="https://github.com/user-attachments/assets/8bd104e8-d557-4805-bf95-3524012c124a" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 19 08" src="https://github.com/user-attachments/assets/e79e0b70-a8a3-4cde-adf3-a07e18fa6cd7" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 19 35" src="https://github.com/user-attachments/assets/fc66955c-a022-46ec-98f6-a9f3dd26e80c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 20 03" src="https://github.com/user-attachments/assets/63eefc0f-4129-455a-9e78-9f1b1f8c2f25" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 22 31" src="https://github.com/user-attachments/assets/a443d455-b010-470b-b5dd-162ec2a119d4" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 30 17" src="https://github.com/user-attachments/assets/653b82b9-a4c6-4382-8fd1-d733f8bcf351" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 34 32" src="https://github.com/user-attachments/assets/7ff3deef-db9d-4ec5-bc06-209f9072d471" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 36 45" src="https://github.com/user-attachments/assets/f5e2077f-de67-4454-bc6d-3776c42efc0f" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 39 52" src="https://github.com/user-attachments/assets/12e8d6a8-704a-4dab-aa97-fe1ad6215cc5" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 41 15" src="https://github.com/user-attachments/assets/93016ca5-578d-440a-a9ab-077843d2b831" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 42 17" src="https://github.com/user-attachments/assets/8a624175-dc69-4a47-837e-e0228aaaff42" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 43 01" src="https://github.com/user-attachments/assets/b0ccb357-029b-4e01-a16a-f4b3ede6d743" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 43 50" src="https://github.com/user-attachments/assets/08846116-26c1-4c8a-ab79-76744f445612" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 44 39" src="https://github.com/user-attachments/assets/f6eb39ea-8413-46b6-937c-d0344837ab4c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 46 18" src="https://github.com/user-attachments/assets/12ce9da3-5e6b-4073-8293-4f62b7c6c400" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 47 10 (1)" src="https://github.com/user-attachments/assets/d73ba9df-f0d6-47db-af43-227e088c70fc" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 49 34" src="https://github.com/user-attachments/assets/92e98d8f-a8e7-48b0-ba77-cb2464a66ffe" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 17 51 04 (1)" src="https://github.com/user-attachments/assets/ca97ce0b-473d-452e-9801-87f8cdf024fc" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 02 48" src="https://github.com/user-attachments/assets/5869c494-a668-410b-8ef1-bb71667484a5" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 03 45" src="https://github.com/user-attachments/assets/8f3c8400-7a07-4dfd-beff-f99ea4179bf8" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 04 46" src="https://github.com/user-attachments/assets/6586bb37-3ebd-473c-b44c-66c7966765c2" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 06 09" src="https://github.com/user-attachments/assets/0c603601-ed5e-4175-9c46-21db01968855" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 08 09" src="https://github.com/user-attachments/assets/c3f82f60-677a-4f9e-b02b-ef3cf9cc4602" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 08 46" src="https://github.com/user-attachments/assets/cfc16af6-6471-44d6-8423-5b5d6924f05f" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 09 21" src="https://github.com/user-attachments/assets/c6c5ee16-6996-4321-97d7-b92cc854542b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 13 46" src="https://github.com/user-attachments/assets/de2edcd6-545f-4250-b699-28a0e998568e" />
<img width="1600" height="1351" alt="WhatsApp Image 2026-05-23 at 18 16 18" src="https://github.com/user-attachments/assets/5e8f9f9a-2255-4ebf-b218-9d1ee84f4184" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 17 15" src="https://github.com/user-attachments/assets/2b60e316-4058-40ee-847f-6b525d647abb" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 19 06" src="https://github.com/user-attachments/assets/38289c08-9e9d-4875-84eb-15ee88850dee" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 20 19" src="https://github.com/user-attachments/assets/9244c040-fa5a-4ca1-9cc1-addaf00e44e4" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 21 08" src="https://github.com/user-attachments/assets/906fa38a-987b-4891-92d1-7c17b7cf20d2" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 21 41" src="https://github.com/user-attachments/assets/01f9260d-da32-4af7-b9f9-0e957e9f2cbd" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 22 24" src="https://github.com/user-attachments/assets/38daffda-83e5-4027-a527-52a58a97c3a1" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 22 51" src="https://github.com/user-attachments/assets/7e39189c-8d6e-4e93-9ab4-706483dede00" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 23 33" src="https://github.com/user-attachments/assets/c75f19a3-0d5d-4ab1-a16c-98ea49d0b59a" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 27 11" src="https://github.com/user-attachments/assets/ae73e3e6-acb2-4d30-91bc-62bcdd38e0aa" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 27 48" src="https://github.com/user-attachments/assets/51b58156-6d65-470d-bc1d-353ce0a6db6d" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 28 22" src="https://github.com/user-attachments/assets/6afc0944-b199-429d-b2c5-c1a89e25e25b" />
<img width="1600" height="1136" alt="WhatsApp Image 2026-05-23 at 18 29 08" src="https://github.com/user-attachments/assets/b4d69d95-727f-456f-834b-d785cb008beb" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 29 32" src="https://github.com/user-attachments/assets/c42ad8d0-583b-414d-9ab8-22fb2ec52fdf" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 34 52" src="https://github.com/user-attachments/assets/1b69d82b-fdf3-4480-9dcf-98fd6ceddc48" />
<img width="1600" height="1098" alt="WhatsApp Image 2026-05-23 at 18 35 44" src="https://github.com/user-attachments/assets/575da19c-f3eb-438a-b894-e05eee5678b7" />
<img width="1600" height="1102" alt="WhatsApp Image 2026-05-23 at 18 36 44" src="https://github.com/user-attachments/assets/0b023324-9c2e-4e91-819a-e1b4884f5238" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 37 21" src="https://github.com/user-attachments/assets/b56ae962-262f-40fc-aadf-774444d5184f" />
<img width="1600" height="1087" alt="WhatsApp Image 2026-05-23 at 18 38 10" src="https://github.com/user-attachments/assets/392064c1-136a-40b6-ad0d-fa20c0f2c8e5" />
<img width="1600" height="1103" alt="WhatsApp Image 2026-05-23 at 18 39 09" src="https://github.com/user-attachments/assets/18fb09dc-abb7-4f02-bc6e-bad8ff999a51" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 18 39 52" src="https://github.com/user-attachments/assets/f52ba04a-5dff-4dc6-a06b-912d84e02170" />
<img width="1600" height="1100" alt="WhatsApp Image 2026-05-23 at 18 40 00" src="https://github.com/user-attachments/assets/98bb8244-8621-41a3-9238-d2e8d854590c" />






