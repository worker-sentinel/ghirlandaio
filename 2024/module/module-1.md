# Panduan Instalasi Arch Linux (Bahasa Indonesia)

Dokumen ini adalah panduan untuk menginstal Arch Linux menggunakan live system dari media instalasi resmi.

Arch Linux dikenal sebagai distribusi Linux minimalis dan fleksibel. Sistem ini memberi pengguna kontrol penuh terhadap paket dan konfigurasi yang dipasang.

---

# Persiapan Sebelum Instalasi

##  Download ISO Arch Linux
# Panduan Membuat Installer Arch Linux (USB Bootable)

Media instalasi ini akan memberikan *live environment* yang digunakan untuk melakukan instalasi sistem operasi Arch Linux ke komputer.

## Persiapan Dasar
- **File ISO:** Unduh versi terbaru dari [archlinux.org/download](https://archlinux.org).
- **USB Drive:** Minimal kapasitas 2GB (direkomendasikan 4GB atau lebih).
- **Peringatan:** Proses ini akan menghapus **seluruh data** di dalam USB drive.

---

## Metode Pembuatan

### A. Menggunakan Terminal (Linux & macOS)
Metode ini menggunakan perintah `dd` yang sangat stabil.

1. Hubungkan USB dan cari nama drive-nya:
   ```bash
   lsblk  # Linux
   diskutil list # macOS
   ```
2. Jalankan perintah *flashing* (Pastikan target `of=/dev/sdX` benar):
   ```bash
   sudo dd bs=4M if=/jalur/ke/archlinux.iso of=/dev/sdX status=progress && sync
   ```
   *Ganti `sdX` dengan identitas USB kamu (misal: `sdb`). Jangan gunakan nomor partisi seperti `sdb1`.*

### B. Menggunakan Rufus (Windows)
1. Buka **Rufus**.
2. Pilih USB drive pada menu **Device**.
3. Klik **Select** dan pilih file ISO Arch Linux yang sudah diunduh.
4. Klik **Start**.
5. Jika muncul jendela peringatan, pilih opsi **Write in DD Image mode**.

### C. Menggunakan BalenaEtcher (Grafis - Semua OS)
1. Buka **balenaEtcher**.
2. Klik **Flash from file** dan pilih file ISO.
3. Klik **Select target** dan tentukan USB drive.
4. Klik **Flash!**.

---

## Langkah Booting
1. Colokkan USB ke komputer yang akan diinstal.
2. Masuk ke **BIOS/UEFI** (tekan `F2`, `F12`, `Del`, atau `Esc` saat booting).
3. Ubah urutan boot agar USB berada di posisi teratas.
4. Pilih menu **Arch Linux install medium** saat layar menu muncul.

---

## Perintah Awal di Live Environment
Setelah masuk ke konsol (`root@archiso`), jalankan perintah berikut untuk memastikan sistem siap:


# Memeriksa Mode Boot

```bash
cat /sys/firmware/efi/fw_platform_size
```

### Hasil

- `64` → UEFI 64-bit
- `32` → UEFI 32-bit
- Error → BIOS/Legacy mode

### Penjelasan
Penting untuk menentukan jenis bootloader dan partisi yang digunakan.

---

# 6. Menghubungkan ke Internet

Cek interface jaringan:

```bash
ip link
```

Tes koneksi:

```bash
ping ping.archlinux.org
```

### Penjelasan
Internet diperlukan karena Arch mengunduh paket langsung dari mirror online saat instalasi.

Untuk WiFi gunakan:

https://wiki.archlinux.org/title/Iwd
---

# 7. Sinkronisasi Waktu

```bash
timedatectl
```

### Penjelasan
Jam sistem harus benar agar verifikasi SSL dan package signature tidak gagal.

---

# Partisi Disk

Lihat disk:

```bash
fdisk -l
```

Partisi disk:

```bash
fdisk /dev/the_disk_to_be_partitioned
```

Contoh:

```bash
fdisk /dev/nvme0n1
```

---

# Struktur Partisi yang Disarankan

## UEFI

| Mount Point | Fungsi |
|---|---|
| `/boot` | EFI Partition |
| `swap` | Virtual memory |
| `/` | Root system |

## BIOS Legacy

| Mount Point | Fungsi |
|---|---|
| `swap` | Virtual memory |
| `/` | Root system |

---

# Format Partisi

```bash
mkfs.ext4 /dev/nvme0n1p1
```

### Penjelasan
Membuat filesystem ext4 pada partisi root.

---

## Membuat Swap

```bash
mkswap /dev/nvme0n1p2
```

Aktifkan swap:

```bash
swapon /dev/swap_partition
```

### Penjelasan
Swap digunakan sebagai memori cadangan ketika RAM penuh.

---

## Format EFI Partition

```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

### Penjelasan
EFI partition harus menggunakan FAT32.

---

# Mount Filesystem

Mount root:

```bash
mount /dev/root_partition /mnt
```

Mount EFI:

```bash
mount --mkdir /dev/efi_system_partition /mnt/boot
```

### Penjelasan
Partisi harus dipasang (mount) sebelum sistem diinstal.

---

# Instalasi Sistem Dasar

```bash
pacstrap -K /mnt base linux linux-firmware base base-devel
```

### Penjelasan

- `base` → paket inti sistem
- `linux` → kernel Linux
- `linux-firmware` → firmware hardware

---

# Membuat fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Penjelasan
fstab menentukan partisi mana yang otomatis dimount saat boot.

---

# Masuk ke Sistem Baru (Chroot)

```bash
arch-chroot /mnt
```

### Penjelasan
Mengubah root shell ke sistem Arch yang baru dipasang.

---

# Mengatur Timezone

```bash
ln -sf /usr/share/zoneinfo/Area/Location /etc/localtime
```

Contoh Indonesia:

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Sinkronkan hardware clock:

```bash
hwclock --systohc
```

---

# Localization

Generate locale:

```bash
locale-gen
```

Isi `/etc/locale.conf`:

```ini
LANG=en_US.UTF-8
```

### Penjelasan
Mengatur bahasa dan format regional sistem.

---

# Hostname

Isi file:

```text
/etc/hostname
```

Contoh:

```text
myarchpc
```

### Penjelasan
Hostname adalah nama komputer di jaringan.

---

# Generate Initramfs

```bash
mkinitcpio -P
```

### Penjelasan
Membuat image boot awal Linux.

---

# Password Root

```bash
passwd
```

### Penjelasan
Mengatur password administrator/root.

---

# Install Bootloader

Pilihan umum:

- GRUB
- systemd-boot
- rEFInd

Dokumentasi:

https://wiki.archlinux.org/title/Arch_boot_process

---

# Reboot

Keluar dari chroot:

```bash
exit
```

Unmount:

```bash
umount -R /mnt
```

Reboot:

```bash
reboot
```

Lepas USB installer setelah restart.

---

# Resource Resmi

- https://archlinux.org/
- https://archlinux.org/download/
- https://wiki.archlinux.org/
- https://bbs.archlinux.org/
