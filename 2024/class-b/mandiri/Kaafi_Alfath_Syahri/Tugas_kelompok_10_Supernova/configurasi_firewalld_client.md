# Konfigurasi Zona WiFi

## Membuat Zona WiFi

Buat zona firewall baru dengan nama wifi:

```bash
sudo firewall-cmd --permanent --new-zone=wifi
```

## Menambahkan Sumber IP ke Zona WiFi

Tambahkan jaringan yang akan menggunakan zona wifi:

```bash
sudo firewall-cmd --permanent --zone=wifi --add-source=15.15.5.4/24
```

## Membuka Port HTTP

Izinkan akses layanan web melalui port 80/TCP:

```bash
sudo firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```

## Menerapkan Konfigurasi

Muat ulang Firewalld agar seluruh perubahan dapat diterapkan:

```bash
sudo firewall-cmd --reload
```

## Verifikasi Konfigurasi

Periksa kembali konfigurasi zona wifi untuk memastikan pengaturan telah berhasil diterapkan:

```bash
sudo firewall-cmd --zone=wifi --list-all
```
