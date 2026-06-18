# Dokumentasi Penginstalan Docker Swarm, Apache, dan OpenDocMan
## Nama: Fatma Ramadhani
## NIM: 12402051050157
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum

## Install Linux-lts
(Cara nya sama seperti kelompok 8 tetapi beda dibagian kernel linux hardened, nah dikelompok ini kita palai linux lts yaaaaa)
Nah, habis itu kita langsung ke terminal managernya dengan command1:
## 1. Install docker
```
sudo su
masukin password
systemctl enable docker
systemctl start docker
systemctl status docker
```
## 2. Instalasi docker swarm 
```
ip a
docker swarm init --advertise-addr (ip manager)
docker node ls
```
 ## 3. Memberikan Label node
 ```
docker node update \
  --label-add role=frontend \
  khai (sebagai manager)
docker node update \
  --label-add role=backend \
  fatma (sebagai worker)
```
```
docker node inspect khai (manager) --pretty
docker node inspect fatma (worker) --pretty
```

## 4. Buat repository Opendocman
```
sudo mkdir -p /opt/stacks
cd /opt/stacks

git clone https://github.com/opendocman/opendocman.git

cd opendocman
```
## 5. Kita Generate environment 
```
./scripts/generate-env-secrets.sh
```
## 6. Lalu, kita Build image
```
docker build -t opendocman:pathched

docker images | grep opendocman
```
## 7. Next step, kita Overlay Network
```
docker network create \
--drive overlay \
opendocman-net

docker networks ls
```
## 8. Lalu, kita buat Direktori
```
sudo mkdir -p /srv/opendocman/mysql
sudo mkdir -p /srv/opendocman/files
sudo mkdir -p /srv/opendocman/config


sudo chmod -R 755 /srv/opendocman
```
## 9. Kita check Stack yml
```
nvim stack.yml
```
Insert
```
version: "3.9"

services:

  db:
    image: mariadb:10.11

    env_file:
      - .env

    volumes:
      - /srv/opendocman/mysql:/var/lib/mysql

    networks:
      - opendocman-net

    deploy:
      replicas: 1
      placement:
        constraints:
          - node.labels.role == backend

  opendocman:
    image: opendocman:patched

    env_file:
      - .env

    volumes:
      - /srv/opendocman/files:/var/www/html/files-data
      - /srv/opendocman/config:/var/www/html/docker-configs

    ports:
      - target: 80
        published: 8080
        protocol: tcp
        mode: ingress

    networks:
      - opendocman-net

    deploy:
      replicas: 1
      placement:
        constraints:
          - node.labels.role == frontend

networks:
  opendocman-net:
    external: true
```
## 10 Lalu kita Deploy stack
```
docker stack deploy -c stack.yml opendocman
```
## 11 Dan Verifikasi service
```
docker stack services opendocman
```
## 12 Jangan lupa Install Apache 
```
sudo pacman -S apache
sudo systemctl enable --now httpd
systemctl status httpd
```
## 13 Kita anukan Module reverse proxy
```
sudo nvim /etc/httpd/conf/httpd.conf
```
DIBAWAH INI JANGAN DIKOMENTARI (HAPUS HASHTAGNYA YAAAA)
```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule headers_module modules/mod_headers.so
LoadModule rewrite_module modules/mod_rewrite.so
```
## 14 Terus kita buat deh Virtual host opendocman
```
sudo nvim /etc/httpd/conf/conf.d/opendocman.conf
```
LALU INSERT:
```
<VirtualHost *:80>

    ServerName [khaifatma.local.test] (disesuaikan sama keinginan kalian aja yee alias BEBAS)

    ProxyPreserveHost On
    ProxyRequests Off

    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    ErrorLog "/var/log/httpd/opendocman-error.log"
    CustomLog "/var/log/httpd/opendocman-access.log" combined

</VirtualHost>
```
## 15 Lalu kita Include host
```
sudo nvim /etc/httpd/conf/httpd.conf
```
Insert kepaling bawah dengan command
```
Include conf/conf.d/opendocman.conf
```
## 16 Habis itu, kita Konfigurasi Apache
```
sudo apachectl configtest

sudo systemctl restart httpd
```
## 17 Jangan lupa di iniin Firewall nya
```
sudo firewall-cmd --permanent --add-service=http

sudo firewall-cmd --permanent --add-service=https

sudo firewall-cmd --reload
```
## 18 Kita langsung buat DNS atau host
```
(no ip a manager) khaifatma.local.test
```
## 19 Akses web Opwndocman
Kalau sudah, kita langsungkan saja pergi ke firefox punya manager dengan contoh commandnya kek gini:
```
http://khaifatma.local.test/install/setup-config
```
## FINISH
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/6494cda2-2e51-46ce-996e-c01c2b699ed3" />


Disclaimer: dokumentasi ini dibuat oleh dua orang yaitu Khairunnisa Candra dan Fatma Ramadhani, dibuat dengan sangat bertanggung jawab dan lillahi taala.
