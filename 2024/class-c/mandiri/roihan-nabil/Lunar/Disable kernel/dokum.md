# Langkah-Langkah Menonaktifkan Modul Kernel (Blacklist Module)

Panduan ini digunakan untuk mematikan modul tertentu (misalnya `usb-storage`) agar tidak dimuat oleh sistem saat booting.

### 1. Masuk ke Server lewat SSH
Buka terminal dan login ke server target menggunakan akun yang memiliki akses sudo:
```bash
ssh lunarserver@192.168.1.12

```
### 2. Pindah ke Hak Akses Root
Setelah berhasil login, masuk sebagai root agar bisa mengubah konfigurasi sistem resmi:
```bash
sudo su

```
### 3. Cek Modul yang Aktif (Opsional)
Untuk memastikan modul yang ingin dimatikan sedang berjalan atau tidak di memory, gunakan lsmod:
```bash
lsmod | grep usb_storage

```
### 4. Buat File Konfigurasi Modprobe
Buat file baru di direktori /etc/modprobe.d/ untuk memblokir modul. Di sini kita menggunakan text editor nano:
```bash
nano /etc/modprobe.d/disable-module.conf

```
Tambahkan dua baris berikut di dalam file tersebut:
```text
install usb-storage /bin/false
blacklist usb-storage

```
*Simpan perubahan dengan menekan Ctrl + O, lalu keluar dengan Ctrl + X.*
### 5. Perbarui Ramdisk / Initcpio Image
Agar konfigurasi ini diterapkan pada tingkat kernel sejak awal proses booting, perbarui semua preset initcpio yang ada menggunakan mkinitcpio:
```bash
mkinitcpio -P

```
