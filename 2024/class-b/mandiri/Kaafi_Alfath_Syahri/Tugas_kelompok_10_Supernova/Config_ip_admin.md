# Konfigurasi Koneksi Jaringan dan Pembuatan User Operator

## Memeriksa Status Perangkat Jaringan

Untuk melihat status seluruh jaringan yang tersedia, jalankan perintah berikut:

```bash
nmcli device status
```

## Menampilkan Informasi IP

Perintah berikut digunakan untuk menampilkan informasi terkait konfigurasi jaringan:

```bash
ip
```

## Membuat Koneksi Ethernet Baru

Tambahkan koneksi Ethernet dengan konfigurasi alamat IP statis:

```bash
nmcli connection add type ethernet ifname enp2s0 con-name "admin-connection" ipv4.method manual ipv4.addresses 15.15.5.2/24 ipv4.gateway 15.15.5.1 ipv4.dns 8.8.8.8
```

## Mengaktifkan Koneksi Secara Otomatis Saat Login

Tambahkan perintah berikut ke file `.bash_profile` agar koneksi aktif secara otomatis saat pengguna masuk ke sistem:

```bash
echo "nmcli connection up admin-connection" >> /home/nama_user/.bash_profile
```

## Membuat User Baru

Buat akun pengguna baru dengan nama **operator**:

```bash
sudo useradd -m operator
```

## Mengatur Password User Operator

Atur kata sandi untuk akun **operator**:

```bash
sudo passwd operator
```

Masukkan password baru, kemudian konfirmasi password tersebut saat diminta oleh sistem.

## Keluar dari Server

Setelah seluruh konfigurasi selesai dilakukan, keluar dari sesi terminal dengan perintah:

```bash
exit
```
