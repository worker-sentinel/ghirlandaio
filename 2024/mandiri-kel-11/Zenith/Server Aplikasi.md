

## 1. Menghubungkan Server ke Jaringan Wi-Fi

Masuk ke utilitas pengelola Wi-Fi.

```bash
iwctl
```

Lihat daftar jaringan Wi-Fi yang tersedia.

```bash
station wlan0 get-networks
```

Hubungkan perangkat ke jaringan Wi-Fi.

```bash
station wlan0 connect "nama_wifi"
```

Keluar dari utilitas Wi-Fi.

```bash
exit
```

---

## 2. Menginstal Podman Compose

Instal Podman Compose yang digunakan untuk menjalankan container.

```bash
sudo pacman -S podman-compose
```

---

## 3. Membuat Direktori Konfigurasi Container

Buat direktori konfigurasi container.

```bash
mkdir -p ~/.config/containers
```

Periksa direktori yang tersedia.

```bash
ls -la
```

Masuk ke direktori konfigurasi container.

```bash
cd ~/.config/containers/
```

---

## 4. Menginstal Wget

Instal utilitas wget untuk mengunduh file dari internet.

```bash
sudo pacman -S wget
```

Masukkan password ketika diminta.

---

## 5. Mengekstrak Source Code SLiMS

Ekstrak file arsip yang telah diunduh.

```bash
bsdtar -xzf master.zip
```

Periksa hasil ekstraksi.

```bash
ls
```

Ubah nama folder hasil ekstraksi menjadi lebih sederhana.

```bash
mv docker-compose-for-slims-master slims
```

Pastikan folder berhasil diubah.

```bash
ls
```

Masuk ke direktori SLiMS.

```bash
cd slims
```

Lihat isi direktori aplikasi.

```bash
ls
```

---

## 6. Konfigurasi Docker Compose

Buka file konfigurasi container untuk dilakukan penyesuaian.

```bash
nvim docker-compose-redis-elasticsearch.yaml
```

---

## 7. Mengatur Hak Akses Direktori Aplikasi

Berikan hak akses penuh pada direktori aplikasi SLiMS.

```bash
sudo chmod -R 777 app/slims
```

Masukkan password ketika diminta.

---

## 8. Konfigurasi Kernel Linux

Buka file konfigurasi sysctl.

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

Terapkan konfigurasi yang telah dibuat.

```bash
sudo sysctl --system
```

---

## 9. Konfigurasi Storage Container

Buka konfigurasi penyimpanan Podman.

```bash
nvim ~/.config/containers/storage.conf
```

---

## 10. Konfigurasi Registry Container

Buka konfigurasi registry container.

```bash
sudo nvim /etc/containers/registries.conf
```

Masukkan password ketika diminta.

---

## 11. Menjalankan Layanan SLiMS

Masuk ke mode root.

```bash
sudo su
```

Jalankan seluruh container yang diperlukan oleh SLiMS.

```bash
podman compose up -d
```

Periksa status container yang sedang berjalan.

```bash
podman ps
```

---

## 12. Keluar dari Mode Root

Keluar dari akun root setelah proses instalasi selesai.

```bash
exit
```
