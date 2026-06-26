# Dokumentasi Cek Server Aplikasi (1)
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Salfa Firyal Hasanah
3. Fatma Ramadhani
4. Aditya Pangruwating Dhiyu
5. Fauzan Azhiimi
6. Ahmad Hafiz Baihaqi
## Step pertama, cek kondisi IP server
Di awal kita pastiin dulu server udah dapat alamat IP yang benar. Nantinya IP ini dipakai buat akses aplikasi dari browser.

```bash
ip a
```

---

## Selanjutnya, cek status container
Sekarang kita lihat dulu apakah container yang dipakai aplikasi sudah berjalan atau belum.

```bash
podman ps -a
```

---

## Lalu, jalankan Podman Compose
Kalau ternyata container belum aktif, kita bisa langsung menjalankan semua service yang ada di file compose.

```bash
podman compose up -d
```

---

## Kemudian, matikan firewall sementara
Kalau aplikasi masih belum bisa diakses dari jaringan, firewall bisa dimatikan sementara untuk memastikan apakah masalahnya berasal dari firewall atau bukan.

```bash
sudo systemctl stop firewalld
```

---

## Setelah itu, cek lagi status container
Abis firewall dimatikan, kita cek lagi apakah semua container masih berjalan dengan normal.

```bash
podman ps -a
```

---

## Lanjut, pastikan alamat IP server
Sekali lagi cek IP server untuk memastikan alamat yang akan dipakai saat mengakses aplikasi melalui browser.

```bash
ip a
```

---

## Terakhir, keluar dari server
Kalau semua proses pengecekan sudah selesai, tinggal keluar dari sesi terminal.

```bash
exit
```
