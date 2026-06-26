# Install Podman Compose
```
sudo pacman -S podman-compose
```

# Buat Folder Konfigurasi
```
mkdir -p .config/galactic
```
```
cd .config/galactic
```

# Download Source SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

Karena di os admin kelompok saya belum install wget jadi install terlebih dahulu
```
sudo pacman -S wget unzip
```
lalu download source slims lagi
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
unzip master.zip
```

# Rename Folder
```
mv docker-compose-for-slims-master compose
```
```
cd compose
```

# Konfigurasi Port Non-Privileged
```
sudo nvim /etc/sysctl.d/13-custom.conf
```
Tambahkan konfigurasi
```
net.ipv4.ip_unprivileged_port_start=80
```
```
sudo sysctl –system
```

# Mengganti File Docker Compose
```
mv docker-compose.yaml docker-compose.yaml.bck
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
Tambahkan konfigurasi
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

# Mengaktifkan Layanan Podman
```
sudo systemctl enable --global podman
```

# Konfigurasi Storage
```
nvim .config/galactic/storage.conf
```
Tambahkan konfigurasi
```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

```
sudo chmod -R 777 app/slims
```
```
exit
```
Di bagian ini kelompok kami error jadinya membuat asciinema dan dokumentasi baru

