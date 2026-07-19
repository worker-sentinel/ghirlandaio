# langkah awalnya 
## mengsinkronkan waktu

```
sudo timedatectl set-ntp true
```
> Deskripsi: Perintah ini mengaktifkan Network Time Protocol (NTP) pada sistem operasi. Sistem akan secara otomatis mencari server waktu di internet untuk menyamakan jamnya.

> Alasan: Kubernetes sangat sensitif terhadap perbedaan waktu (clock skew). Jika waktu antar node (Server dan Agent) berbeda bahkan hanya beberapa detik, proses autentikasi menggunakan token/sertifikat SSL bisa gagal, menyebabkan node tidak bisa bergabung atau komunikasi antar-komponen menjadi error.


## mengaktifkan sinkronisasi waktu otomatis
```
sudo timedatectl set-timezone Asia/Jakarta
```

> Deskripsi: Mengubah zona waktu sistem ke Asia/Jakarta (Waktu Indonesia Barat / WIB).

> Alasan: Memudahkan manusia (operator/admin) dalam membaca log aktivitas server. Jika terjadi masalah, Anda tidak perlu pusing mengonversi waktu UTC ke waktu lokal Anda untuk mengetahui kapan tepatnya masalah itu terjadi.

## cek status
```
sudo timedatectl
```
 > Deskripsi: Menampilkan status pengaturan waktu saat ini, termasuk waktu lokal, waktu universal (UTC), zona waktu yang aktif, dan apakah NTP sudah aktif atau belum.

> Alasan: Sebagai langkah verifikasi (double-check) untuk memastikan bahwa dua perintah sebelumnya (NTP dan Zona Waktu) sudah sukses diterapkan sebelum melangkah ke instalasi K3s.

## region control
## lewat admin kita ssh region control untuk dijadikan control-plane
```
curl -sfL https://get.k3s.io | sh -s server --disable traefik
```

> Deskripsi: Mengunduh script instalasi resmi K3s, lalu mengeksekusinya dengan mode server (sebagai control-plane), serta menambahkan parameter --disable traefik untuk mencegah K3s menginstal Traefik secara otomatis sebagai Ingress Controller bawaan.

> Alasan: 
> * server: Menandakan node ini yang memegang kendali (menyimpan database cluster, mengatur scheduling pod, dll).
> * --disable traefik: Traefik bawaan K3s terkadang versinya outdated atau fiturnya kurang sesuai dengan kebutuhan produksi. Menonaktifkannya di awal memberi Anda kebebasan untuk menginstal Ingress Controller lain yang lebih populer atau sesuai kebutuhan nantinya (seperti NGINX Ingress, Kong, atau Traefik versi terbaru via Helm).

## langsung install dari script resminya

```
sudo cat /var/lib/rancher/k3s/server/node-token 
```
> Deskripsi: Membaca isi file node-token yang digenerate otomatis oleh K3s setelah proses instalasi selesai. Token ini berisi string rahasia yang panjang.

> Alasan: Token ini bertindak sebagai kunci keamanan. Ketika Anda ingin menambahkan node baru (Worker Node/Agent) ke dalam cluster ini, node tersebut wajib menyertakan token ini agar Control-Plane tahu bahwa node baru tersebut legal dan diizinkan bergabung ke dalam cluster.

## untuk cek token yang akan digunakan
> contoh output:
```
K1038fb91da987e64ac1d98b948bc00520b51f8ef4f1dca73ea14a2391a4c1fb76c::server:e08f645e38888f1dfab0766a250aceee
```

## cek status region
## harus dari laptop admin diregion control
```
 sudo k3s kubectl get nodes   
```
> Deskripsi: Memanggil perintah kubectl (alat baris perintah standar Kubernetes yang sudah dibundel di dalam K3s) untuk melihat daftar semua node yang terhubung ke cluster beserta statusnya.

> Alasan: Untuk memverifikasi apakah master node Anda sudah berhasil berjalan dengan status Ready. Perintah ini juga digunakan nantinya untuk memastikan apakah Worker Node yang Anda tambahkan di langkah berikutnya sudah berhasil terdeteksi dan aktif di dalam cluster.


