# Install Aplikasi (SLiMS)

## Install Compose
```
sudo pacman -S podman-compose
```
```
mkdir (nama untuk folder)
```
## Cara ke foldernya
```
cd (nama folder yang udah dibuat)
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Kalo ga ada wget, install dulu wget nya
```
sudo pacman -S wget unzip
```
## Pas udah install wgetnya, baru balik ke command buat install SLiMS
## Extract Zip
```
unzip master.zip
```
```
ls
```
Hasil yang udah diunzip 
```
docker-compose-for-slims-master
```
## Untuk Rename
```
mv docker-compose-for-slims-master [nama rename-nya] (misal: compose)
```
## Cek apakah rename nya berhasil/ tidak
```
ls
```
## Masuk ke folder yang udah di rename
```
cd (nama folder yang udah di rename)
```
> Deskripsi: Berpindah ke dalam folder yang baru saja diganti namanya.

> Alasan: Semua langkah konfigurasi selanjutnya (edit file compose, atur permission, dan menjalankan container) harus dilakukan dari dalam folder ini karena file docker-compose.yaml ada di sini.
# Set Kernel 
## Butuh permission untuk kernel 
```
sudo nvim /etc/sysctl.d/[nama file].conf
```
## Masuk mode insert
```
net.ipv4.ip_unprivileged_port_start=80
```
```
sudo sysctl --system
```
## Pastiin ada command yang tadi ditulis waktu mode insert
```
ls
```
## Rename
```
mv docker-compose.yaml docker-compose.yaml.[nama rename nya]
```
> Deskripsi: Mengganti nama file docker-compose.yaml bawaan menjadi nama lain, misalnya docker-compose.yaml.bak, sehingga file ini "dinonaktifkan" sementara namun tidak dihapus.
> Alasan: Repositori SLiMS biasanya menyediakan lebih dari satu file compose (misalnya versi database MySQL biasa dan versi dengan Redis). Dengan mengganti nama file default, kita menyimpannya sebagai cadangan sambil menyiapkan file compose lain yang ingin dipakai.
## Rename lagi
```
mv docker-compose-redis.yaml docker-compose.yaml
```
> Deskripsi: Mengganti nama docker-compose-redis.yaml menjadi docker-compose.yaml, sehingga Podman Compose akan membaca file ini sebagai konfigurasi utama secara otomatis.

> Alasan: Podman Compose secara default mencari file bernama docker-compose.yaml. Karena versi yang ingin dipakai adalah versi dengan Redis (biasanya untuk performa cache yang lebih baik), maka file tersebut perlu dijadikan file utama dengan cara mengganti namanya.
```
nvim docker-compose.yaml
```
> Deskripsi: Membuka file docker-compose.yaml menggunakan editor teks Nvim untuk diperiksa atau disesuaikan isinya (misalnya port, nama volume, atau environment variable).

> Alasan: Konfigurasi bawaan dari GitHub mungkin perlu disesuaikan dengan kondisi server, seperti port yang dipakai atau lokasi penyimpanan data, sebelum container dijalankan.
## Bikin hak akses buat folder dan file yang ada di dalam folder
```
sudo chmod -R 777 app/slims
```
> Deskripsi: Mengubah hak akses (permission) folder app/slims beserta seluruh isinya secara rekursif (-R) menjadi dapat dibaca, ditulis, dan dieksekusi oleh siapa saja (777).

> Alasan: Container SLiMS perlu menulis file (misalnya file upload, log, atau cache) ke dalam folder ini. Jika permission folder terlalu ketat, container akan gagal menulis data dan aplikasi bisa error. Perlu dicatat, izin 777 ini cukup longgar dari sisi keamanan, sehingga sebaiknya hanya dipakai di lingkungan development/latihan, bukan di server produksi.
## Running Compose (tanpa sudo)
```
podman compose up -d
```
> Deskripsi: Menjalankan seluruh container yang didefinisikan dalam docker-compose.yaml dalam mode detached (-d), artinya container berjalan di latar belakang tanpa mengunci terminal.

> Alasan: Ini adalah langkah untuk mulai menjalankan SLiMS. Dilakukan di sini sebagai percobaan awal untuk melihat apakah ada error konfigurasi sebelum lanjut ke pengaturan tambahan seperti registry dan storage.
```
sudo nvim /etc/containers/registries.conf
```
sudo nvim /etc/subuid
```
## Cek subgid
```
sudo nvim /etc/subgid
```
cd ..
```
## Bikin storage simpanan
```
nvim storage.conf
```
> Deskripsi: Membuat/mengedit file storage.conf, yaitu file yang menentukan bagaimana dan di mana Podman menyimpan data image serta layer container.

> Alasan: Pengaturan storage yang tepat memastikan Podman menyimpan data container di lokasi yang benar dan dengan driver penyimpanan yang sesuai (misalnya overlay), sehingga container dapat berjalan stabil dan efisien.
```
sudo systemctl enable --global podman
```
> Deskripsi: Mengaktifkan layanan (service) Podman agar berjalan secara otomatis untuk semua user pada saat sistem menyala (boot), bukan hanya untuk satu user saja.

> Alasan: Dengan opsi --global, container yang dibuat oleh user biasa (rootless) tetap bisa otomatis berjalan kembali setelah server di-restart, tanpa perlu login manual dan menjalankan ulang perintah podman compose up.
```
cd  [nama direktori]
```
## Running Podman Compose
```
podman compose up -d
```
## Cek Containers
```
podman ps -a
```
## Masukin port SLiMS 
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
sudo firewall-cmd --reload
```
## Cek IP Server (Inet)
```
ip a
```
## Buka browser buat cek apakah SLiMS udah masuk atau belum, masukin IP Server
``` 
http://[IP_Server]:80
```
