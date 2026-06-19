
## 1. Mengecek Git
git --version                                                                                                                                                         
git version 2.54.0          

## 2. Mengecek Curl
```bash
curl –version
```
## 3. Pengujian Koneksi Internet
```bash
ping 8.8.8.8
```
## 4. Pengecekan SSH 
ssh -v
```
# Tahap 4 - Sinkronisasi Repository

## Memperbarui Database Paket

```bash
pacman -Sy
```

# Tahap 5 - Instalasi Paket

## Menginstal Paket yang Dibutuhkan

```bash
pacman -S wget curl openssh htop podman
```

# Tahap 6 - Verifikasi Hasil Instalasi

## Verifikasi Podman

Perintah:

```bash
podman –version
```
pacman -Q networkmanager

pacman -Q openssh

exit 
