# Konfigurasi Firewall (Admin)

## 1. Koneksi Admin ke Server

Masuk ke server menggunakan SSH:

```bash
ssh F123@10.38.202.71
```

Masukkan password server saat diminta.

---

## 2. Melihat Konfigurasi Firewall Saat Ini

```bash
sudo firewall-cmd --list-all-zones
```

Masukkan password server saat diminta.

---

## 3. Menghapus Service DHCP dari Zona `nm-shared`

```bash
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dhcp
sudo firewall-cmd --reload
```

---

## 4. Memverifikasi Konfigurasi Firewall

```bash
sudo firewall-cmd --list-all-zones
```

---

## 5. Menghapus Service DNS dan SSH dari Zona `nm-shared`

```bash
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dns
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=ssh
sudo firewall-cmd --reload
```

---

## 6. Menambahkan Service HTTP pada Zona `public`

```bash
sudo firewall-cmd --zone=public --add-service=http --permanent
```

---

## 7. Membuka Port yang Diperlukan

```bash
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

### Keterangan Port

| Port | Protokol | Fungsi |
|--------|-----------|----------|
| 80 | TCP | HTTP (Web Server) |
| 443 | TCP | HTTPS (Web Server SSL/TLS) |
| 3306 | TCP | MariaDB/MySQL Database |

---

## 8. Keluar dari Sesi Server

```bash
exit
```
