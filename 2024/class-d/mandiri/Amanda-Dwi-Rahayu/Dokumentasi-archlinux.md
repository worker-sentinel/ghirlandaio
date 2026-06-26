# Dokumentasi Instal ArchLinux
## Nama: Amanda Dwi Rahayu
## NIM: 12402051030100
## Kelas: 4D
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum

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

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 06" src="https://github.com/user-attachments/assets/21441c08-b53c-4c1b-af45-c93ca4571431" />

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 06 (1)" src="https://github.com/user-attachments/assets/e350ea6c-4611-4c95-971f-5ac53e0ca5d9" />

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 08" src="https://github.com/user-attachments/assets/bdca0d4e-c8ac-43cb-a3e9-47ee5f3abb8c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 09" src="https://github.com/user-attachments/assets/b2094ff6-8357-4032-977f-1c7e495733b4" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 09 (1)" src="https://github.com/user-attachments/assets/5fa22183-f96a-4f24-8e83-b650ef735b2f" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 10" src="https://github.com/user-attachments/assets/a6b6d837-380e-4bd3-ac04-17232b7794f3" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 10 (1)" src="https://github.com/user-attachments/assets/da99543f-a7ef-432b-8e57-5b79fb6f1ac1" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 11" src="https://github.com/user-attachments/assets/04740c63-39d8-448c-88f1-781625e7c3f9" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 12" src="https://github.com/user-attachments/assets/85c5e19f-44e9-4067-9c3c-e1ad74168e63" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 13" src="https://github.com/user-attachments/assets/f8e356d4-4dea-44af-95a5-12cc9b3dc684" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 14" src="https://github.com/user-attachments/assets/2c1a6c94-3a2e-4ae0-8aa5-373bb7e87f30" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 15" src="https://github.com/user-attachments/assets/d9b16810-255f-47e5-bf7b-9b1120329253" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 15 (1)" src="https://github.com/user-attachments/assets/44426cc9-7473-40fb-ba58-ae38258f10c8" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 16" src="https://github.com/user-attachments/assets/cc5f7253-dace-4c56-8486-e37b7e3e5873" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 17" src="https://github.com/user-attachments/assets/a6aa8704-7fa0-4a61-b0ca-1dcefa1168e1" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 19" src="https://github.com/user-attachments/assets/b03c9b38-d38c-4de1-a4cf-e0c3fe30e7d0" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 20" src="https://github.com/user-attachments/assets/97c39722-a015-4842-b7ae-19944a65942c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 20 (1)" src="https://github.com/user-attachments/assets/28b8f1b0-e205-4c66-8b62-f4e7a40bc51a" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 21" src="https://github.com/user-attachments/assets/37f91aee-34f8-4647-9ff0-e4cdb838b59d" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 23" src="https://github.com/user-attachments/assets/d04f6382-b412-4735-b15b-20fa53b8723e" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 25" src="https://github.com/user-attachments/assets/ea93e596-9682-421b-88eb-e2f3053774fe" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 26" src="https://github.com/user-attachments/assets/2b6c7330-6b83-46dc-a953-82190c581949" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 27" src="https://github.com/user-attachments/assets/8a2c5f79-5b2d-42f0-bb72-33974a73ce51" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 27 (1)" src="https://github.com/user-attachments/assets/ac894023-43cb-4e86-a771-ad8482199033" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 28" src="https://github.com/user-attachments/assets/10aebf35-752b-4c89-9286-10dc48773500" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 29 (1)" src="https://github.com/user-attachments/assets/060bbbba-5c57-4b57-a78c-06c16182042b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 26" src="https://github.com/user-attachments/assets/f2d678ca-1e9b-40c0-b55a-9bad8c4506b1" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 27" src="https://github.com/user-attachments/assets/5dcd379c-7a85-4285-bccf-d0b798ca7e46" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 27 (1)" src="https://github.com/user-attachments/assets/2795a2df-6f07-42bb-9aba-400bf6a4c680" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 28" src="https://github.com/user-attachments/assets/9a6e54e6-21da-4d88-ba79-47294bbac766" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 29" src="https://github.com/user-attachments/assets/3cc0bb6f-f276-435c-8072-765a13de6d0f" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 29 (1)" src="https://github.com/user-attachments/assets/b20e61a3-efa3-4396-be30-178bc20f2f02" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 30" src="https://github.com/user-attachments/assets/c21357b6-2387-483d-9d98-395114c96f23" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 33" src="https://github.com/user-attachments/assets/7902065b-8b6d-4f81-96b6-b7d9380cfdcd" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 35" src="https://github.com/user-attachments/assets/e66c8ed6-f981-48d7-8ad3-4ca6d675cd10" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 39" src="https://github.com/user-attachments/assets/34522308-a641-4036-b365-6503034748ab" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 41" src="https://github.com/user-attachments/assets/2163d796-7b27-455f-b568-e12bbc8cc301" />
<img width="1600" height="1021" alt="WhatsApp Image 2026-05-23 at 09 07 42" src="https://github.com/user-attachments/assets/d5da7686-1de9-4333-b2e2-a5facf20efed" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 43" src="https://github.com/user-attachments/assets/88eb0167-84ec-4c75-83e3-d07f9824f58f" />
<img width="1600" height="1351" alt="WhatsApp Image 2026-05-23 at 09 07 44 (1)" src="https://github.com/user-attachments/assets/5463bdbf-06c9-4703-af8f-1f8adc7ad081" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 44 (2)" src="https://github.com/user-attachments/assets/1a933437-afb3-466f-ab94-7f5a3ad5e27c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 45" src="https://github.com/user-attachments/assets/999b5fdb-d0bd-4bd1-89a0-ca10159c0320" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 46" src="https://github.com/user-attachments/assets/0e94f0a0-969d-4c7c-b66c-805305bdbe25" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 47" src="https://github.com/user-attachments/assets/96707f05-a290-49a6-96e9-0b6136657302" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 47 (1)" src="https://github.com/user-attachments/assets/e92207dd-bd60-4e71-98a7-2b2f7c761154" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 48" src="https://github.com/user-attachments/assets/0407c9f9-cc30-4478-be60-1a121bcaa7b8" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 48 (1)" src="https://github.com/user-attachments/assets/f8d17624-5b26-493f-889d-cd5b732a4c85" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 49" src="https://github.com/user-attachments/assets/141da027-cc75-48b2-83ce-f227d8b311f3" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 49 (1)" src="https://github.com/user-attachments/assets/028cd143-11e7-4c48-bf5e-6b90179b4a7b" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 50" src="https://github.com/user-attachments/assets/05322bfb-c60c-4825-a924-4a6706154d68" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-23 at 09 07 50" src="https://github.com/user-attachments/assets/fdc9165b-ea9f-49ba-aa69-723f5092ce56" />
<img width="1600" height="1102" alt="WhatsApp Image 2026-05-23 at 09 07 51 (1)" src="https://github.com/user-attachments/assets/9335c16f-7cae-4496-a6ae-5e2a8bde76bf" />
<img width="1600" height="1100" alt="WhatsApp Image 2026-05-23 at 09 07 53" src="https://github.com/user-attachments/assets/ecb218d4-df05-4a08-8a40-2a7d3f017029" />






