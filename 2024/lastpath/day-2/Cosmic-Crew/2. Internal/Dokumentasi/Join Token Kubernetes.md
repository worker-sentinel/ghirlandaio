# Connect Kubernetes Internal

## 1. Proses Koneksi dan Pengecekan Awal

Bagian ini adalah langkah awal untuk masuk ke server target dan memeriksa konfigurasi jaringan.

```
ssh kel10@192.168.1.18
```

Melakukan remote akses menggunakan SSH ke server dengan IP ``` 192.168.1.18 ``` menggunakan username ``` kel10 ```

```
password
```

Memasukkan kata sandi untuk autentikasi user kel10 secara manual.

```
sudo su
```

Mengubah hak akses dari user biasa ``` kel10 ``` menjadi Root / Superuser karena konfigurasi sistem memerlukan hak akses tertinggi.

``` masukkan password ``` input manual

```
ip a
```

Menampilkan seluruh konfigurasi antarmuka jaringan (IP address, MAC address, status interface) pada server target. Biasanya digunakan untuk memastikan IP ``` 192.168.1.18 ``` sudah terpasang dengan benar.

```
exit
```

Keluar dari sesi Root kembali menjadi user biasa ``` kel10 ```.

```
exit
```

Menutup koneksi SSH dan keluar dari server ``` 192.168.1.18 ``` kembali ke komputer lokal/master.

```
ip a
```

Memeriksa IP address pada komputer lokal/master (tempat menjalankan perintah saat ini).

## 2. Instalasi dan Registrasi K3s Agent (Worker Node) 

masuk kembali ke server ``` .18 ``` untuk menginstal K3s dengan peran sebagai Agent (Worker Node).

```
ssh kel10@192.168.1.18
```

Melakukan remote akses menggunakan SSH ke server dengan IP ``` 192.168.1.18 ``` menggunakan username ``` kel10 ```

```
password
```

Memasukkan kata sandi untuk autentikasi user kel10 secara manual.

```
sudo su
```

Mengubah hak akses dari user biasa ``` kel10 ``` menjadi Root / Superuser karena konfigurasi sistem memerlukan hak akses tertinggi.

```
curl -sfL https://get.k3s.io | sudo sh -s - agent --server https://192.168.1.17:6443 --token <masukkan_token>
```

Mengunduh script instalasi K3s secara otomatis (curl) lalu mengeksekusinya (sh). Perintah ini mendaftarkan server ini sebagai Agent yang berkiblat ke Master Server di IP ``` 192.168.1.18 ``` lewat port ``` 6443 ``` menggunakan token keamanan agar diizinkan bergabung ke cluster.

## 3. Manajemen Layanan K3s Agent

Setelah instalasi, memeriksa apakah layanan agent berjalan dengan baik.

```exit
systemctl status k3s-agent.service
```

Memeriksa status operasional dari layanan K3s Agent. Di sini bisa dilihat apakah statusnya ``` active (running) ``` atau terjadi error.

```
systemctl restart k3s-agent.service
```

Memuat ulang atau menyalakan kembali layanan K3s Agent. Biasanya dilakukan jika ada perubahan konfigurasi atau layanan sempat error/stuck.

```
systemctl status k3s-agent.service
```

Memastikan kembali apakah setelah di-restart, layanannya sudah berjalan normal tanpa kendala.

```
exit
```
``` CTRL+D ```

Keluar dari Root dan mengakhiri sesi SSH di server ``` .18 ```.

## 4. Pengecekan Node dari Master Server

Sekarang kembali di Master Server (atau komputer lokal yang memiliki akses kubectl).

```
kubectl get nodes
```

Menampilkan daftar semua node (baik master maupun worker) yang terhubung di dalam cluster Kubernetes. Di tahap ini, disini memastikan apakah node ``` 192.168.1.18 ``` tadi sudah berstatus ``` Ready ```.

## 5. Investigasi Konfigurasi Internal K3s

Kembali masuk ke server ``` .18 ``` karena mendeteksi sesuatu atau ingin memeriksa konfigurasi load balancer bawaan K3s.


```
ssh kel10@192.168.1.18
```

Melakukan remote akses menggunakan SSH ke server dengan IP ``` 192.168.1.18 ``` menggunakan username ``` kel10 ```

```
password
```

Memasukkan kata sandi untuk autentikasi user kel10 secara manual.

```
sudo su
```

Mengubah hak akses dari user biasa ``` kel10 ``` menjadi Root / Superuser karena konfigurasi sistem memerlukan hak akses tertinggi.

``` masukkan password ``` input manual

```
cat /var/lib/rancher/k3s/agent/etc/k3s-agent-load-balancer.json
```

Membaca isi file konfigurasi JSON milik load balancer internal K3s Agent. File ini berisi informasi bagaimana agent (worker) tersebut mendistribusikan trafik atau menghubungi master node (server ``` .18 ```).


```
exit
```

Keluar dari sesi Root kembali menjadi user biasa ``` kel10 ```.

```
exit
```

Menutup koneksi SSH dan keluar dari server ``` 192.168.1.18 ``` kembali ke komputer lokal/master.

```
kubectl get nodes
```

Pengecekan akhir untuk memastikan seluruh node di cluster tetap aman dan terkoneksi dengan benar.

```
exit
```

Menutup terminal atau sesi aktif yang sedang digunakan saat itu.
