# Konfigurasi Firewall Server

## Koneksi Admin ke Server

Masuk ke server menggunakan SSH:

```bash
ssh F123@10.38.202.71
```

Masukkan password server ketika diminta.

---

## 1. Melihat Konfigurasi Firewall Saat Ini

Menampilkan konfigurasi firewall yang sedang aktif:

```bash
sudo firewall-cmd --list-all-zones
```

Masukkan password server ketika diminta.

---

## 2. Menghapus Service DHCP dari Zona `nm-shared`

Hapus layanan DHCP dari zona `nm-shared`:

```bash
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dhcp
sudo firewall-cmd --reload
```
---

## 3. Memverifikasi Konfigurasi Firewall

Periksa kembali konfigurasi firewall untuk memastikan perubahan telah diterapkan:

```bash
sudo firewall-cmd --list-all-zones
```

---

## 4. Menghapus Service DNS dan SSH dari Zona `nm-shared`

Hapus layanan DNS dan SSH dari zona `nm-shared`:

```bash
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dns
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=ssh
sudo firewall-cmd --reload
```

---

## 5. Menambahkan Service HTTP pada Zona `public`

Tambahkan layanan HTTP ke zona `public`:

```bash
sudo firewall-cmd --zone=public --add-service=http --permanent
```

---

## 6. Membuka Port pada Zona `public`

Buka port yang diperlukan:

```bash
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```
---

## 7. Keluar dari Sesi Server

Untuk mengakhiri koneksi SSH:

```bash
exit
```



