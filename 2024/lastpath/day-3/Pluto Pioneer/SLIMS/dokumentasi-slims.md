# Install Aplikasi SLiMS
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Fatma Ramadhani
3. Salfa Firyal Hasanah
4. Muhammad Fauzan Azhiimi
5. Aditya Pangruwating Dhiyu
6. Ahmad Hafiz Baihaqi

## Menginstall paket podman-compose 
```
sudo pacman -S podman-compose
```
Command ini dipakai buat install podman-compose, yaitu tools yang nantinya dipakai buat ngejalanin beberapa container sekaligus dari file docker-compose.yaml.
```
Masukan pass
```
```
Proceed wih installation? Y
```
**Membuat direktrori baru**
```
mkdir (nama direktori)
```
Bikin folder baru yang bakal dipakai buat nyimpen semua file proyek SLiMS.
**Masuk ke dalam direktori baru**
```
cd (nama direktori)
```
Masuk ke folder yang tadi udah dibuat biar semua file hasil install ada di situ.

## Menginstall apk wget dan unzip 
```
sudo pacman -S wget unzip
```
```
Proceed with installation? Y
```
Command ini menginstall dua paket sekaligus, yaitu wget yang dipakai buat download file dari internet lewat terminal, dan unzip yang dipakai buat nge-ekstrak file ZIP yang nanti kita download.
## Mengunduh file master ZIP docker-compose SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
Di tahap ini kita download file Docker Compose SLiMS langsung dari repository GitHub. Opsi -c berguna kalau proses download sempat terputus, jadi nanti bisa dilanjutin tanpa harus mulai dari awal.
## Mengekstrak berkas master.zip yang telah diunduh
```
unzip master.zip
```
Setelah file berhasil didownload, command ini dipakai buat nge-ekstrak isi file master.zip supaya semua file proyeknya bisa diakses.
## Melihat daftar berkas
```
ls
```
Command ini dipakai buat ngecek apakah hasil ekstraksi tadi udah muncul di dalam folder yang lagi kita buka.
## Mengubah nama direktori hasil ekstrasi SLiMS dari nama bawaan GitHub menjadi nama proyek baru, namanya bebas
```
mv docker-compose-for-slims-master (nama proyek baru)
```
Folder hasil download dari GitHub masih pakai nama bawaan, jadi kita ganti dulu sesuai nama proyek yang diinginkan supaya lebih gampang dikenali.
## Masuk ke dalam direktori proyek baru
```
cd (nama proyek baru)
```
Setelah nama folder diganti, kita masuk ke dalam folder proyek tersebut buat lanjut ke proses konfigurasi.
## Membuat file konfigurasi sistem
```
sudo su
```
```
nvim /etc/sysctl.d/eleven.conf
```
Di sini kita bikin file konfigurasi baru menggunakan Neovim. Setelah file terbuka, tekan i buat masuk ke mode insert, lalu tambahin baris berikut.
klik i trs tambahin **net.ipv4.ip_unprivileged_port_start = 80 klik** Ketik :wq
Konfigurasi ini dipakai supaya aplikasi yang jalan tanpa hak akses root tetap bisa menggunakan port 80. Setelah selesai, tekan Esc, lalu ketik :wq buat nyimpen perubahan dan keluar dari editor.
```
sysctl –system
```
Command ini dipakai buat me-reload semua konfigurasi kernel yang ada di folder sysctl.d, jadi perubahan yang tadi dibuat bisa langsung diterapkan tanpa perlu restart.
## Menyiapkan file komposisi kontainer
```
mv docker-compose.yaml docker-compose.yaml.pluto-compose
```
File compose bawaan diubah namanya dulu supaya tetap tersimpan sebagai cadangan dan nggak langsung ketimpa.
```
mv docker-compose-redis.yaml docker-compose.yaml
```
Setelah itu file compose yang mau dipakai diganti namanya menjadi docker-compose.yaml, karena Podman Compose secara otomatis akan membaca file dengan nama tersebut.
```
nvim docker-compose.yaml
```
Command ini dipakai buat membuka file konfigurasi container. Di dalam file ini kita mengatur database MySQL, aplikasi SLiMS, port yang akan digunakan, volume penyimpanan data, dan juga network yang menghubungkan antar container.
>klik i lalu isi dengan ini:
```
version: "3.7"
services: 
    db:
        image: mysql:5.7
        restart: always
        networks: 
            slims-net:
                ipv4_address: 172.18.1.3
        container_name: slims-db
        env_file: 
            - db_default.env
        command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION --max_allowed_packet=1024M
        volumes:
            - "./dbdata:/var/lib/mysql"
        #ports:
        #    - "3306:3306"
    app01:
        image: slimsofficial/slims:latest
        restart: always
        networks: 
            slims-net:
                ipv4_address: 172.18.1.11
        container_name: slims-app
        ports:
            - "80:80"
            - "443:443"
        volumes:
            - "./app:/var/www/html"
networks: 
    slims-net:
        name: slims-net
        ipam:
            driver: default
            config:
              - subnet: "172.18.1.0/24"
```
## Memberikan hak akses penuh
```
chmod -R 777 app/slims
```
Command ini memberikan hak akses penuh ke folder app/slims, sehingga container bisa membaca, menulis, dan mengubah file yang dibutuhkan selama aplikasi berjalan.
## Running Compose
```
podman compose up -d
```
Setelah semua konfigurasi selesai, command ini dijalankan buat membangun sekaligus menjalankan semua container berdasarkan isi file docker-compose.yaml. Opsi -d artinya container akan berjalan di background.
```
sudo nvim /etc/containers/registries.conf
```
Command ini membuka file konfigurasi registry Podman. Setelah file terbuka, tambahkan konfigurasi berikut.
**isi dengan**
```
unqualified-search-registries = ["docker.io"]
```
Konfigurasi ini bikin Podman otomatis mengambil image dari Docker Hub kalau nama registry-nya nggak ditulis secara lengkap.
**lalu kita akan,**
```
nvim slims-test.yaml
```
Di tahap ini kita bikin file manifest Kubernetes yang nantinya dipakai buat menjalankan database MariaDB, aplikasi SLiMS, deployment, dan service. File ini juga mengatur supaya aplikasi bisa diakses lewat NodePort 30088.
```
apiVersion: v1
kind: Service
metadata:
  name: slims-db-service
spec:
  ports:
  - port: 3306
  selector:
    app: slims-db
---
apiVersion: v1
kind: Pod
metadata:
  name: slims-db-pod
  labels:
    app: slims-db
spec:
  containers:
  - name: mariadb
    image: mariadb:10.6
    env:
    - name: MARIADB_ROOT_PASSWORD
      value: "slimsrootpass"
    - name: MARIADB_DATABASE
      value: "slims_db"
    - name: MARIADB_USER
      value: "slims_user"
    - name: MARIADB_PASSWORD
      value: "slimspassword"
    ports:
    - containerPort: 3306
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: slims-web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: slims-web
  template:
    metadata:
      labels:
        app: slims-web
    spec:
      containers:
      - name: slims
        image: slimsofficial/slims:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: slims-web-service
spec:
  type: NodePort
  selector:
    app: slims-web
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30088
```

## Cek containers
```
podman ps
```
Command ini dipakai buat memastikan container yang dibutuhkan udah berhasil jalan.
```
podman ps -a
```
Kalau mau lihat semua container, termasuk yang statusnya berhenti, bisa pakai command ini.
## Masukan port SliMS
```
firewall-cmd --zone=public --add-port=80/tcp –permanent
```
Command ini dipakai buat membuka akses ke port 80 supaya aplikasi SLiMS bisa diakses dari jaringan lain.
```
firewall-cmd –reload
```
Setelah aturan firewall ditambahkan, firewall perlu di-reload supaya konfigurasi barunya langsung aktif.
## Cek IP a
```
ip a
```
Command ini dipakai buat melihat alamat IP server yang nanti digunakan buat mengakses aplikasi SLiMS lewat browser.
## Kembali ke user 
```
exit
```
Command ini digunakan buat keluar dari mode root dan kembali ke user sebelumnya.
## Keluar dari SSH server
```
logout
```
Kalau semua proses di server udah selesai, command ini dipakai buat mengakhiri sesi SSH.
## Keluar dari root PC admin
```
exit
```
Command ini dipakai buat menutup sesi terminal yang sedang digunakan.
## Berhentikan asciinema
```
Ctrl + d
```
Tekan kombinasi tombol ini buat mengakhiri proses perekaman terminal yang tadi dijalankan menggunakan Asciinema.
```
asciinema upload nama file.cast
```
## Buka di browser buat cek apakah SLiMS sudah masuk atau belum
```
http://[IP Server]:80
```
Kalau semua langkah sebelumnya udah berhasil, tinggal buka browser lalu akses alamat IP server di port 80. Kalau instalasinya berhasil, halaman utama SLiMS bakal langsung muncul dan aplikasi udah bisa diakses dari perangkat client.
# Tampilan Slims dari kacamata Client
<img width="1280" height="542" alt="image" src="https://github.com/user-attachments/assets/a1bbea55-056f-4f91-921d-c6b1fb71e33c" />

# Tampilan dari hp client
<img width="1080" height="2294" alt="image" src="https://github.com/user-attachments/assets/06db70c5-d9bb-40f4-a7d8-1c3dffd0f643" />
