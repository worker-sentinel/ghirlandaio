# Generate Token Kubernetes

# Root Accsess
```
Sudo su
```
>Berpindah dari user biasa ke user root.

>Agar memiliki hak akses penuh untuk menjalankan perintah perintah yang memerlukan izin administrator, seperti menginstal aplikasi atau mengubah konfigurasi sistem.

# Connect Wifi
```
nmtui
```
>Buka network manager TUI Untuk konek ke jaringan secara interaktif.

>Digunakan untuk menghubungkan komputer ke jaringan WiFi atau LAN secara interaktif tanpa harus mengetik banyak perintah.

# Cek koneksi internet
```
ping 8.8.8.8
```
>Mengecek apakah komputer sudah terhubung ke internet.

# Cek Interface Jaringan
```
ip a
```
>Menampilkan informasi semua antarmuka jaringan

>Untuk melihat alamat IP, nama interface jaringan, dan status koneksi yang sedang digunakan.

```
nmcli divece disconnect enp0s31f6
```
>Memutus koneksi pada perangkat jaringan dengan nama enp0s31f6.

>Biasanya digunakan untuk memutus koneksi LAN atau Ethernet, misalnya saat ingin berpindah ke jaringan lain atau melakukan pengujian koneksi.

```
ping 8.8.8.8
```
>Mengecek apakah koneksi internet masih tersedia setelah jaringan diputus.

>Untuk memastikan apakah komputer masih memiliki akses internet atau memang sudah terputus.

```
clear
```
>Membersihkan tampilan terminal.

>Agar layar terminal menjadi lebih rapi sehingga perintah berikutnya lebih mudah dilihat.

# Install K3s
```
curl -sfL https://get/K3s.io | sh -
```
>Mengunduh lalu langsung menjalankan script instalasi K3s, digunakan untuk menginstal K3s, yaitu versi ringan dari Kubernetes yang digunakan untuk mengelola container dan cluster dengan lebih sederhana.

>-s : Menjalankan proses tanpa banyak menampilkan informasi.

>-f : Menghentikan proses jika terjadi error.

>-L : Mengikuti alamat baru jika terjadi pengalihan (redirect).

# Ambil Node Token
```
cat /var/lib/rancher/k3s/node-token
```
>Menampilkan isi file node-token.

>Token ini digunakan ketika ingin menambahkan node lain ke dalam cluster K3s sehingga node tersebut dapat bergabung dengan server utama.

# Install Kubectl
```
pacman -S kubectl
```
>Menginstal aplikasi kubectl menggunakan package manager pacman.

>kubectl adalah alat yang digunakan untuk mengelola cluster Kubernetes melalui terminal, seperti melihat node, pod, deployment, dan resource lainnya.

```
kubectl get nodes
```
>Menampilkan daftar node yang ada di dalam cluster Kubernetes.

>Digunakan untuk memastikan bahwa instalasi K3s berhasil dan node sudah terdaftar serta berstatus Ready sehingga siap digunakan.

# Keluar dari root
```
exit
```
>Keluar dari mode root.

```
CTRL + D
```
>Untuk mengakhiri asciinema
