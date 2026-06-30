# CONNECT WIFI

```
iwctl
```
> untuk mengelola koneksi WIFI 
```
station wlan0 scan
```
> untuk memindai jaringan WIFI di sekitar
```
station wlan0 get-networks
```
> untuk menampilkan daftar WIFI yang ditemukan
```
station wlan0 connect "nama_jaringan"
```
> untuk menghubungkan ke jaringan wifi, kemudian dilanjutkan dengan memasukkan password
```
exit
```
untuk keluar dari iwctl
## TEST JARINGAN
```
ping google.com
```
> untuk memastikan sudah terhubung WIFI
#  PARTISI

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi]
```
> untuk membentuk layout yang akan diinstall
### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 3G [EFI system]
system = 50G [linux filesystem]
```
> membuat partisi EFI sebesar 3 GB dan membuat partisi utama Linux sebesar 50 GB
#   SETUP LUKS

```
cryptsetup luksFormat --sector-size 4096 /dev/partisi_system
```
> untuk menyesuaikan format partisi dengan file system yang sudah dibuat
```
cryptsetup luksOpen /dev/partisi_system [nama partisi]
```
> untuk membuka partisi yang sudah diformat
```
pvcreate /dev/mapper/[nama partisi]
```
> membuat physical volume
```
vgcreate [nama grup] /dev/mapper/[nama partisi]
```
> membuat volume grup
