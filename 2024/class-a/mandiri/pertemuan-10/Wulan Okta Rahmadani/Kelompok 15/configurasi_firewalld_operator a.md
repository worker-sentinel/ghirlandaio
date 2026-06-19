# Konfigurasi Zona Operator

## Membuat Zona Baru

Buat zona firewall baru dengan nama **operator**:

```bash
firewall-cmd --permanent --new-zone=operator
```

## Menambahkan Sumber IP ke Zona Operator

Daftarkan alamat IP yang akan menggunakan zona **operator**:

```bash
firewall-cmd --permanent --zone=operator --add-source=15.15.5.3
```

## Menambahkan Layanan SSH

Izinkan akses **SSH** pada zona **operator**:

```bash
firewall-cmd --permanent --zone=operator --add-service=ssh
```

## Membuka Port Redis

Izinkan koneksi pada port **6379/TCP**:

```bash
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```

## Membuka Port HTTP

Izinkan layanan web melalui port **80/TCP**:

```bash
firewall-cmd --permanent --zone=operator --add-port=80/tcp
```

## Membuka Port HTTPS

Izinkan layanan web aman melalui port **443/TCP**:

```bash
firewall-cmd --permanent --zone=operator --add-port=443/tcp
```

## Menerapkan Perubahan Konfigurasi

Setelah seluruh konfigurasi selesai, muat ulang Firewalld agar perubahan diterapkan:

```bash
firewall-cmd --reload
```
