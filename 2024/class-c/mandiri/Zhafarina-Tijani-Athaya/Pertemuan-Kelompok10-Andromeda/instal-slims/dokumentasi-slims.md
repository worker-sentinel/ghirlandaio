## Instalisasi Slims

### Download Podman-Compose
```
sudo pacman -S podman-compose
```

### Buat Folder Config
```
mkdir -p .config/andromeda
```
### Masuk ke folder
```
cd .config/andromeda
```
### Install wget
```
pacman -S wget
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
### Install unzip
```
pacman -S unzip
```
### Extract Zip
```
cd master.zip
```
```
unzip master.zip
```
```
mv docker-compose-for-slims-master compose
```
```
cd compose
```
### Konfigurasi Kernel
```
nvim /etc/sysctl.d/13.costum.conf
```
lalu diisikan
```
net.ipv4.ip_unprivileged_port_start=80
```
Agar user non-root bisa menggunakan port 80

```
sysctl --system
```
Terapkan konfigurasi sysctl tanpa reboot

```
mv docker-compose.yaml docker-compose.yaml.bck
```
Backup file docker-compose asli

```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
Isi dengan:

```
version: "3.7"
services:
    db:
        image: mysql:5.7
        restart: always
        networks:
            - slims-net
        container_name: slims-db
        env_file:
            - db_default.env
        command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION --max_allowed_packet=1024M
        volumes:
            - "./dbdata:/var/lib/mysql"
        ports:
            - "127.0.0.1:3306:3306"
    redis:
        image: redis:latest
        restart: always
        networks:
            - slims-net
        container_name: redis
        ports:
            - "127.0.0.1:6379:6379"
    app01:
        image: slimsofficial/slims:latest
        restart: always
        networks:
            - slims-net
        container_name: slims-app
        ports:
            - "80:80"
            - "443:443"
        volumes:
            - "./app:/var/www/html"
            - "./conf/php/php.ini:/usr/local/etc/php/conf.d/php.ini"
networks:
    slims-net:
        name: slims-net
        ipam:
            driver: default
```

### Change Mode
```
chmod -R 777 app/slims
```
### Podman compose
```
podman compose up -d
```

### Edit file Registries
```
nvim /etc/containers/registries.conf
```
lalu diubah pada


<img width="266" height="24" alt="Screenshot 2026-06-19 121624" src="https://github.com/user-attachments/assets/5e50ce7b-6ca3-488a-a529-e3bef9b7c9aa" />

```
nvim /etc/subuid
```
pastikan isinya 
```
100000:65536
```
```
nvim /etc/subgid
```
```
nvim /strorage.conf
```
tambahkan


<img width="98" height="56" alt="Screenshot 2026-06-19 122601" src="https://github.com/user-attachments/assets/6a192a53-4a18-4ad8-9abd-6b51321ed7d8" />


### Service podman
```
systemctl enable--global podman
```

### Jalankan container 
```
podman compose up -d
```

### Jalankan ulang semua container 
```
podman ps -a
```

## Selesai dan Buka Browser
http://(ip server):80 
