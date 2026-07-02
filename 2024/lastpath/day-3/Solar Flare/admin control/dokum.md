# Dokumentasi install aplikasi slims

## Install Compose
```
sudo pacman -S podman-compose
```
> Deskripsi: menginstal podman-compose adalah tool yang memungkinkan kita menjalankan banyak container sekaligus menggunakan satu file konfigurasi, tetapi dengan mesin Podman bukan Docker.

> Alasan: SLiMS dijalankan melalui beberapa container (aplikasi dan database) yang saling terhubung. Tanpa podman-compose, kita harus menjalankan setiap container satu per satu secara manual, yang jauh lebih ribet dan rawan kesalahan.
## Bikin folder untuk nyimpen file compose
```
mkdir (nama untuk folder)
```
## Cara ke foldernya
```
cd (nama folder yang udah dibuat)
```
> Deskripsi: mkdir membuat folder baru, sedangkan cd memindahkan posisi terminal ke dalam folder tersebut.

> Alasan: Kita butuh satu folder khusus untuk menyimpan seluruh file konfigurasi SLiMS agar rapi dan tidak tercampur dengan file sistem lain. Ini juga memudahkan jika suatu saat SLiMS perlu dihapus atau dipindahkan, karena semua file cukup ada dalam satu folder.
## Download SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
> Deskripsi: Perintah ini mengunduh file konfigurasi resmi SLiMS untuk Docker/Podman dari repositori GitHub dalam bentuk file ZIP.

> Alasan: File ini berisi docker-compose.yaml beserta seluruh struktur folder yang sudah disiapkan oleh developer SLiMS, sehingga kita tidak perlu menulis konfigurasi container dari nol. Opsi -c berguna untuk melanjutkan unduhan jika sebelumnya sempat terputus.
## Kalo ga ada wget, install dulu wget nya
```
sudo pacman -S wget unzip
```
> Deskripsi: Menginstal dua tool sekaligus yaitu wget untuk mengunduh file dari internet lewat terminal dan unzip untuk mengekstrak file berformat ZIP.

> Alasan: Kedua tool ini adalah kebutuhan yang dibutuhkan pada langkah sebelumnya dan sesudahnya. Tanpa wget, proses unduh di langkah 3 tidak bisa dijalankan, dan tanpa unzip, file ZIP yang sudah diunduh tidak bisa dibuka.
## Pas udah install wgetnya, baru balik ke command buat install SLiMS
## Extract Zip
```
unzip master.zip
```
> Deskripsi: mengekstrak isi file master.zip menjadi folder dan file-file aslinya.

> Alasan: File yang diunduh dari GitHub berbentuk kompresi ZIP, sehingga perlu diekstrak dulu agar file konfigurasi di dalamnya bisa diakses dan digunakan.
## Cek list folder
```
ls
```
> Deskripsi: Menampilkan daftar file dan folder yang ada di direktori saat ini.

> Alasan: Digunakan untuk memastikan proses ekstraksi berhasil dan untuk melihat nama folder hasil ekstrak

Hasil yang udah diunzip 
```
docker-compose-for-slims-master
```
## Untuk Rename
```
mv docker-compose-for-slims-master [nama rename-nya] (misal: compose)
```
> Deskripsi: Menampilkan daftar file dan folder yang ada di direktori saat ini.

> Alasan: Digunakan untuk memastikan proses ekstraksi berhasil dan untuk melihat nama folder hasil ekstrak.
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
> Deskripsi: Membuat file konfigurasi kernel baru yang mengatur batas bawah nomor port yang boleh digunakan oleh user biasa (non-root). Nilai 80 berarti user biasa diizinkan memakai port mulai dari 80 ke atas. Perintah sudo sysctl --system diperlukan untuk memuat ulang (reload) seluruh pengaturan kernel, termasuk file yang baru dibuat, agar perubahannya langsung aktif.

> Alasan: port di bawah 1024 hanya boleh dipakai oleh root demi alasan keamanan. Karena kita ingin menjalankan Podman secara rootless (tanpa sudo), pengaturan ini diperlukan agar container yang berjalan sebagai user biasa tetap bisa memakai port 80.
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
> Deskripsi: Membuka file konfigurasi yang menentukan darisumber mana Podman boleh menarik (pull) image container, misalnya Docker Hub atau Quay.io.

> Alasan: Jika file compose SLiMS mengambil image dari registry tertentu tanpa menyebutkan nama registry lengkap, Podman perlu tahu ke mana harus mencari image tersebut. Tanpa konfigurasi ini, proses pull image bisa gagal atau salah sasaran.
## Cek subuid
```
sudo nvim /etc/subuid
```
## Cek subgid
```
sudo nvim /etc/subgid
```
> Deskripsi: Membuka file yang berisi rentang User ID (UID) dan Group ID (GID) tambahan yang dialokasikan untuk user tertentu.

> Alasan: Podman rootless menggunakan teknik user namespace, di mana proses di dalam container "mengira" dirinya root, padahal sebenarnya dipetakan ke UID/GID biasa di luar container. File subuid dan subgid inilah yang menyediakan rentang ID untuk pemetaan tersebut. Jika user belum terdaftar di kedua file ini, container rootless bisa gagal berjalan.
## Keluar dari folder
```
cd ..
```
> Deskripsi: Berpindah satu tingkat ke folder induk (parent directory) dari folder saat ini.

> Alasan: Konfigurasi storage Podman (langkah berikutnya) biasanya berada di luar folder project SLiMS, sehingga perlu keluar dulu dari folder tersebut.
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
> Deskripsi: Kembali masuk ke folder project SLiMS yang berisi file docker-compose.yaml.

> Alasan: Semua pengaturan tambahan (registry, subuid/subgid, storage) sudah selesai di luar folder, sehingga sekarang perlu kembali ke dalam folder project untuk benar-benar menjalankan aplikasinya.
## Running Podman Compose
```
podman compose up -d
```
> Deskripsi: Menjalankan kembali seluruh container SLiMS sesuai konfigurasi yang sudah diperbaiki, dalam mode latar belakang.

> Alasan: Setelah semua prasyarat (port, permission, registry, subuid/subgid, storage) sudah diatur dengan benar, langkah ini adalah eksekusi akhir yang benar-benar menyalakan aplikasi SLiMS beserta database-nya.
## Cek Containers
```
podman ps -a
```
> Deskripsi: Menampilkan daftar seluruh container beserta statusnya (running, exited, dsb), termasuk yang sedang tidak aktif (-a = all).

> Alasan: Untuk memverifikasi apakah container SLiMS dan database-nya benar-benar berjalan (status "Up"), atau justru gagal (status "Exited") sehingga perlu ditelusuri lebih lanjut penyebabnya.
## Masukin port SLiMS 
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
sudo firewall-cmd --reload
```
> Deskripsi: Perintah pertama menambahkan aturan izin (rule) pada firewall untuk membuka port 80 (protokol TCP) di zona public, secara permanen (tetap berlaku setelah restart). Perintah kedua memuat ulang firewall agar aturan baru langsung aktif.

> Alasan: Meskipun container SLiMS sudah berjalan dan memakai port 80, firewall Linux secara default akan memblokir koneksi masuk ke port tersebut demi keamanan. Tanpa membuka port ini, SLiMS tidak akan bisa diakses dari browser, baik dari komputer sendiri maupun dari komputer lain di jaringan yang sama.
## Cek IP Server (Inet)
```
ip a
```
> Deskripsi: Menampilkan seluruh informasi antarmuka jaringan (network interface) pada server, termasuk alamat IP yang sedang digunakan.

> Alasan: Untuk mengakses SLiMS dari browser, kita butuh tahu alamat IP dari server tempat SLiMS berjalan, terutama jika ingin mengaksesnya dari perangkat lain dalam jaringan yang sama.
## Buka browser buat cek apakah SLiMS udah masuk atau belum, masukin IP Server
``` 
http://[IP_Server]:80
```
> Deskripsi: Alamat (URL) yang diketik di browser untuk membuka halaman SLiMS, dengan [IP Server] diganti alamat IP hasil dari perintah ip a.

> Alasan: Ini adalah langkah verifikasi akhir untuk memastikan seluruh proses instalasi berhasil, yaitu SLiMS benar-benar bisa diakses dan tampil di browser.
