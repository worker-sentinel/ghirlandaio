## ssh untuk server public 
```
ssh username@ip_address
```
> description : 
Melakukan koneksi jarak jauh (remote access) yang terenkripsi ke server publik menggunakan protokol SSH (Secure Shell).

> rationale : 
Memungkinkan admin untuk mengelola, mengonfigurasi, dan mengeksekusi perintah di dalam server jarak jauh secara aman dari komputer lokal tanpa harus berada di depan fisik server.

## mengsinkronkan waktu
```
sudo timedatectl set-ntp true
```
> description : 
Mengaktifkan fitur Network Time Protocol (NTP) untuk sinkronisasi waktu otomatis melalui internet. 

> rationale : 
Memastikan jam sistem selalu akurat secara real-time. Jika waktu server melenceng, komunikasi klaster Kubernetes (K3s) bisa rusak, proses otentikasi token/sertifikat SSL akan gagal, dan pencatatan log audit menjadi tidak valid.

```
sudo timedatectl set-timezone Asia/Jakarta
```
> description : 
Mengubah wilayah zona waktu sistem ke Waktu Indonesia Barat (WIB).

> rationale : 
Menyelaraskan seluruh catatan aktivitas dan timestamp log di server dengan zona waktu lokal tim operasional, sehingga memudahkan proses analisis data jika terjadi insiden.

```
sudo timedatectl
```
> description : 
Menampilkan ringkasan status konfigurasi waktu saat ini (lokal, universal, status NTP, dan zona waktu).

> rationale : 
Digunakan sebagai langkah verifikasi (validasi) untuk memastikan bahwa pengaturan NTP sudah aktif (NTP service: active) dan zona waktu telah berubah dengan benar sebelum masuk ke instalasi aplikasi.

## region public 
> harus dari laptop admin di region control
```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 
```
> description : 
Mengunduh dan mengeksekusi skrip instalasi K3s dengan parameter untuk mendaftarkan server tersebut sebagai Agent (Worker Node) yang terhubung ke Master Node (Control Plane).

> rationale : 
Perintah ini mengotomatiskan pemasangan komponen runtime kontainer dan agen K3s (k3s agent). Menggunakan token keamanan (K3S_TOKEN) dan alamat master (K3S_URL) memastikan bahwa node baru ini terintegrasi ke dalam klaster yang tepat secara aman dan terenkripsi.

```
sudo kubectl label node [hostname_server] role=[rolenya]
```
> description : 
Menempelkan label (tagging) identitas peran (misal: worker, frontend, atau storage) pada node yang baru bergabung.

> rationale : 
Kubernetes menggunakan label untuk mengorganisasi dan mengelompokkan resource. Dengan memberikan label peran, sistem Control Plane dapat mengatur scheduling (penempatan) pod/aplikasi agar berjalan di server yang spesifik dan sesuai dengan peruntukannya.
