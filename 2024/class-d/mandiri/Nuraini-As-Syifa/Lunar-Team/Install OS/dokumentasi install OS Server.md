# Dokumentasi penginstallan penugasan kelompok 9
## Nama: Nuraini As Syifa
## NIM: 12402051030088
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.
### Konfigurasi Koneksi Jaringan:
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect nama_wifi
```
### Pengujian koneksi jaringan
```
ping 1.1.1.1
```
```
ping 8.8.8.8
```
### Melakukan rekaman dengan asciinema
```
asciinema rec [nama_file].cast
```
### Melihat Partisi
```
lsblk
```
### Membagi partisi dengan EFI System dan Linux Filesystem
```
cfdisk /dev/nvme0n1
```
### Mengenkripsi Partisi dengan LUKS
```
cryptsetup luksFormat /dev/nvme0n1p8
```
### Membuka Partisi Enkripsi
```
cryptsetup luksOpen /dev/nvme0n1p8 aw
```
### Membuat LVM
```
pvcreate /dev/mapper/aw
```
### Membuat volume group
```
vgcreate system /dev/mapper/aw
```
### Membuat LV
```
lvcreate -L 8GB system -n root
```
```
lvcreate -L 8GB system -n vars
```
```
lvcreate -L 1GB system -n vlog
```
```
lvcreate -L 1GB system -n vtmp
```
```
lvcreate -L 1GB system -n vaud
```
```
lvcreate -L 15GB system -n home
```
```
lvcreate -l100%FREE system -n podman
```
