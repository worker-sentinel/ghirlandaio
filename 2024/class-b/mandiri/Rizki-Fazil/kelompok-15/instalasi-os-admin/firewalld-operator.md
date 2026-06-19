# Konfigurasi Zona Operator

Buat zona firewall baru dengan nama operator:

```
firewall-cmd --permanent --new-zone=operator
```

## Menambahkan Sumber IP ke Zona Operator

Daftarkan alamat IP yang akan menggunakan zona operator:

```
firewall-cmd --permanent --zone=operator --add-source=<ip_operator>
```

## Menambahkan Layanan SSH

Izinkan akses SSH pada zona operator:

```
firewall-cmd --permanent --zone=operator --add-service=ssh
```

## Membuka Port Redis

Izinkan koneksi pada port 6379/TCP:

```
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```

## Membuka Port HTTP

Izinkan layanan web melalui port 80/TCP:

```bash
firewall-cmd --permanent --zone=operator --add-port=80/tcp
```

## Membuka Port HTTPS

Izinkan layanan web aman melalui port 443/TCP:

```
firewall-cmd --permanent --zone=operator --add-port=443/tcp
```

## Menerapkan Konfigurasi

Setelah seluruh konfigurasi selesai, muat ulang Firewalld agar perubahan diterapkan:

```
firewall-cmd --reload
```

# Referensi
https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-b/mandiri/Kaafi_Alfath_Syahri/Tugas_kelompok_10_Supernova/configurasi_firewalld_operator.md?plain=1
