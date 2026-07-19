# Dokumentasi Instalasi SLIMS

## Download Source SLiMs
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip 
unzip master.zip
```
> ​wget -c ...: Untuk mengunduh file ZIP konfigurasi Docker Compose untuk SLiMS dari GitHub.

> -c (continue): Untuk memastikan jika download terputus, prosesnya bisa dilanjutkan lagi.

> unzip master.zip: Untuk mengekstrak file ZIP yang sudah di-download.

## Rename folder
```
mv docker-compose-for-slims-master compose
cd compose
```
> ​mv ... compose: Untuk mengubah nama folder hasil ekstrak yang panjang menjadi lebih simpel, yaitu compose.

> ​cd compose: Untuk masuk ke dalam folder tersebut untuk melanjutkan proses setup.

## Kernel
```
sudo nvim /etc/sysctl.d/99-custom.conf
```
> Untuk membuka teks editor Neovim untuk mengubah parameter kernel Linux.

```
sudo chmod -R 777 app/slims  
```
> Untuk memberikan hak akses penuh (baca, tulis, eksekusi) ke folder tempat file SLiMS berada.

```
sudo nvim /etcsubid   
```
> 

Masukkan 
```
[user]:100000:65536      
sudo nvim /etcsugid   

dan

[user]:100000:65536
```

## configure storage
```
nvim ~/.config/cleo/storage.conf

[storage]  
driver = "overlay"  
[storage.options.overlay]                                                                                                                                                                 mount_program = ""   
mountopt = "userxattr"
```

```
sudo nvim /etc/containers
```
> Untuk membuka direktori konfigurasi global Podman.

## Menjalankan Container dengan Podman
```
podman pull docker.io/mysql:5.7
```
> Mengunduh (pull) image database MySQL versi 5.7 dari Docker Hub ke komputer lokal menggunakan Podman.

```
podman compose up -d
```
> Untuk membaca file docker-compose.yml yang ada di dalam folder tersebut, lalu membuat dan menjalankan container SLiMS serta MySQL sekaligus.

> -d (detached mode): Untuk membuat container berjalan di latar belakang (background).

```
podman ps -a
```
>  Untuk menampilkan daftar semua container yang ada, baik yang sedang berjalan maupun yang mati dan untuk memastikan apakah container SLiMS dan MySQL sudah aktif dengan status Up.

```
exit
```
> Keluar dari terminal.
