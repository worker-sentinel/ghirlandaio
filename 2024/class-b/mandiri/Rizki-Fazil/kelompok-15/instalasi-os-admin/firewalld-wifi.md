# configurasi firewall Zona WiFi

## Membuat configurasi Zona WiFi

Buat zona firewall baru dengan nama wifi:

```
sudo firewall-cmd --permanent --new-zone=wifi
```

## Menambahkan Sumber IP

Tambahkan jaringan yang akan menggunakan zona wifi:

```
sudo firewall-cmd --permanent --zone=wifi --add-source=<ip_wifi>/<cidr>
```

## Membuka Port HTTP

Izinkan akses layanan web melalui port 80/TCP:

```
sudo firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```

## Menerapkan Konfigurasi

reload Firewalld agar seluruh perubahan dapat diterapkan:

```
sudo firewall-cmd --reload
```

## Verifikasi Konfigurasi

Periksa kembali konfigurasi zona wifi untuk memastikan pengaturan telah berhasil diterapkan:

```
sudo firewall-cmd --zone=wifi --list-all
```
fungsi --list-all adalah untuk menampilkan seluruh firewall pada zone wifi

# Referensi
David aka joshua aka baihaqi
https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-b/mandiri/Kaafi_Alfath_Syahri/Tugas_kelompok_10_Supernova/configurasi_firewalld_client.md?plain=1
