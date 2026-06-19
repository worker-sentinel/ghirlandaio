# Install Aplikasi SLiMS
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Fatma Ramadhani
3. Salfa Firyal Hasanah
4. Muhammad Fauzan Azhiimi
5. Aditya Pangruwating Dhiyu
6. Ahmad Hafiz Baihaqi
## Masuk sebagai superuser (root) local
```
sudo su
```
## Membuka layanan SSH pada firewall local
```
firewall-cmd --zone=public --add-service=ssh –permanent
```
```
firewall-cmd –reload
```
```
systemctl enable sshd
```
```
systemctl start sshd
```
## Melihat konfigurasi IP a
```
Ip a
```
## Mengecek daftar layanan yang diperbolehkan di firewall local
```
firewall-cmd --zone=public --list-service
```
```
ssh (nama server)@192.168.1.71
```
```
Masukan pass
```
## Di dalam server [(nama server)@server]
Menginstall paket podman-compose 
```
sudo pacman -S podman-compose
```
```
Masukan pass
```
```
Proceed wih installation? Y
```
 Membuat direktrori baru
```
mkdir (nama direktori)
```
Masuk ke dalam direktori baru
```
cd (nama direktori)
```
## Menginstall apk wget dan unzip 
```
sudo pacman -S wget unzip
```
```
Proceed with installation? Y
```
## Mengunduh file master ZIP docker-compose SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Mengekstrak berkas master.zip yang telah diunduh
```
unzip master.zip
```
## Melihat daftar berkas
```
ls
```
## Mengubah nama direktori hasil ekstrasi SLiMS dari nama bawaan GitHub menjadi nama proyek baru, namanya bebas
```
mv docker-compose-for-slims-master (nama proyek baru)
```
## Masuk ke dalam direktori proyek baru
```
cd (nama proyek baru)
```
## Membuat file konfigurasi sistem
```
sudo su
```
```
nvim /etc/sysctl.d/eleven.conf
```
>klik i trs tambahin net.ipv4.ip_unprivileged_port_start = 80 klik esc. Ketik :wq
```
sysctl –system
```
## Menyiapkan file komposisi kontainer
```
mv docker-compose.yaml docker-compose.yaml.pluto-compose
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
>klik i trs ada ini
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
## Running Compose
```
podman compose up -d
```
```
sudo nvim /etc/containers/registries.conf
```
## Cek containers
```
podman ps
```
```
podman ps -a
```
## Masukan port SliMS
```
firewall-cmd --zone=public --add-port=80/tcp –permanent
```
```
firewall-cmd –reload
```
## 
Cek IP a
```
ip a
```
## Kembali ke user 
```
exit
```
## Keluar dari SSH server
```
logout
```
## Keluar dari root PC admin
```
exit
```
## Berhentikan asciinema
```
Ctrl + d
```
```
asciinema upload nama file.cast
```
## Buka di browser buat cek apakah SLiMS sudah masuk atau belum
```
http://[IP Server]:80
```
# Tampilan Slims dari kacamata Client
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/1dc7144a-cc22-45d3-9464-f8e718bf3b93" />

**Tampilan dari hp client**

<img width="717" height="1600" alt="image" src="https://github.com/user-attachments/assets/6d55fab1-e1c3-4331-a58c-d7db791bc5eb" />
