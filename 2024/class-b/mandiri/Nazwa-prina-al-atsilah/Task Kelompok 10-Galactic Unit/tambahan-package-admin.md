kami menggunakian linux lts kelompok 9 dengan tampilan desktop, jadi kami menginstall package yang diperlukan

## 1. Mengecek Git
git --version                                                                                                                                                          git version 2.54.0          

## 2. Mengecek Curl
```
curl –version
```
## 3. Pengujian Koneksi Internet
```
ping 8.8.8.8
```
## 4. Pengecekan SSH 
ssh -v
```
# Tahap 4 - Sinkronisasi Repository

## Memperbarui Database Paket

```
pacman -Sy
```

# Tahap 5 - Instalasi Paket

## Menginstal Paket yang Dibutuhkan

```
pacman -S wget curl openssh htop podman
```

# Tahap 6 - Verifikasi Hasil Instalasi

## Verifikasi Podman

command:

```
podman –version
```
pacman -Q networkmanager

pacman -Q openssh

exit
