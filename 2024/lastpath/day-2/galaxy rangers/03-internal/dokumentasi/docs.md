# langkah awalnya 

## mengsinkronkan waktu

```
sudo timedatectl set-ntp true

```
> Deskripsi: 
Perintah ini mengaktifkan fitur Network Time Protocol (NTP) bawaan sistem operasi melalui systemd-timesyncd. Sistem akan otomatis mencari server waktu publik di internet untuk mencocokkan jam.

> Alasan: 
Memastikan jam pada server selalu akurat secara otomatis. Tanpa NTP, waktu lokal server bisa bergeser seiring berjalan waktu, yang berpotensi merusak komunikasi internal cluster K3s.

## mengaktifkan sinkronisasi waktu otomatis

```
sudo timedatectl set-timezone Asia/Jakarta

```
> Deskripsi: 
Mengubah zona waktu (timezone) sistem operasi server menjadi Asia/Jakarta (WIB - Western Indonesian Time / UTC+7).

> Alasan: 
Menyeragamkan zona waktu di seluruh server (Master dan Agent). Hal ini sangat krusial saat Anda melakukan troubleshooting atau membaca log aplikasi; jika zona waktunya sama, Anda bisa melacak urutan kejadian eror secara akurat.

## mengubah zona waktu (timezone) sistem Anda ke Waktu Indonesia Barat (WIB

```
sudo timedatectl

```
> Deskripsi: 
Menampilkan status konfigurasi waktu sistem saat ini, termasuk Waktu Lokal, UTC, Zona Waktu, dan status keaktifan layanan NTP.

> Alasan: 
Digunakan sebagai langkah verifikasi/validasi. Anda perlu memastikan bahwa NTP service: active dan Time zone: Asia/Jakarta sudah diterapkan dengan benar sebelum masuk ke tahap instalasi K3s.

> cek statusnya


## region internal

> harus dari laptop admin di region control

```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 

```
> Deskripsi: 
Mengunduh skrip instalasi resmi K3s, lalu mengeksekusinya dengan mode agent. Perintah ini menyertakan parameter K3S_URL (alamat API Server Master) dan K3S_TOKEN (kunci rahasia dari Server Master).
> Alasan: 
> * agent: Memberitahu skrip bahwa mesin ini bukan penentu kebijakan (master), melainkan pekerja (worker) yang bertugas menjalankan aplikasi (Pod).

> * K3S_URL: Memberikan arah pasif ke mana agent harus melapor dan menerima instruksi kerja.

> * K3S_TOKEN: Berfungsi sebagai alat autentikasi (keamanan). Server Master hanya akan menerima Agent yang membawa token yang cocok demi mencegah server asing tidak dikenal menyusup ke dalam cluster.

```
sudo kubectl label node [hostname_server] role=[rolenya]

``` 
> Deskripsi: 
Memberikan label berupa metadata khusus (pasangan key=value) ke node yang baru saja bergabung menggunakan perintah kubectl.
> Alasan: Secara default, K3s tidak otomatis melabeli peran spesifik pada node agen baru. Pemberian label ini penting karena:
> * Kemudahan Manajemen: Membantu admin mengidentifikasi fungsi node saat mengetik kubectl get nodes (misal: membedakan node di region data vs region public).
> * Penjadwalan Aplikasi (Scheduling): Anda bisa mengatur agar suatu aplikasi (Pod) hanya boleh berjalan di region data menggunakan fitur nodeSelector atau nodeAffinity di dalam file konfigurasi (YAML) Kubernetes Anda.
