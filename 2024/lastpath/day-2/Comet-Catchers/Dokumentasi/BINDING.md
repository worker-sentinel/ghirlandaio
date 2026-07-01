## Sinkronisasi Waktu Sistem
```
sudo timedatectl set-ntp true
```
> Untuk mengaktifkan Network Time Protocol (NTP).

## Mengunduh dan Menginstal K3s
```
sudo curl -sfL https://get.k3s.io | sh -s - server
```
> Untuk mengunduh skrip instalasi K3s dan langsung menjalankannya sebagai Server (Master Node).

## Mengecek Status Layanan K3s
```
sudo systemctl status k3s.service
```
> Untuk memeriksa apakah K3s sudah berjalan dengan baik di latar belakang (background service).

## Masuk sebagai Root (Superuser)
```
sudo su
```
> Untuk Mengubah hak akses terminal kamu menjadi root.

## Berpindah ke Direktori K3s
```
cd /var/lib/
```
```
cd /var/lib/rancher/k3s/server/
```
> Untuk berpindah folder (Change Directory) ke tempat penyimpanan data sensitif milik K3s Server.

## Melihat Isi Direktori
```
ls
```
> Digunakan untuk melihat berkas dan folder apa saja yang ada di dalam direktori.

## Mengambil Token Klaster
```
cat node-token
```
> Untuk membaca dan menampilkan isi dari file node-token.

## Mengecek IP Address
```
ip a
```
> Untuk melihat alamat IP server.

## Mengecek Status Firewall
```
sudo systemctl status firewalld
```
> Untuk memeriksa apakah firewalld aktif atau tidak.

## Membuka Port Kubernetes di Firewall
```
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```
```
sudo firewall-cmd --reload
```
> Untuk mengizinkan data masuk lewat port 6443.

## Mengecek Status Node Kubernetes
```
sudo kubectl get node
```
> Untuk melihat daftar node yang aktif.
