## Install Podman Compose
```
sudo pacman -S podman-compose
```
## Buat Folder Konfigurasi
```
mkdir -p .config/{kelompok10}
```
## Masuk ke Folder 
```
cd .config/{KELOMPOK10}
```
## Download Source SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## 
```
unzip master.zip
```
## Rename Folder
```
mv docker-compose-for-slims-master compose
```
```
cd compose
```
## Buat Konfigurasi Kernel
```
sudo nvim /etc/sysctl.d/13-custom.conf
```
```
sudo sysctl --system
```
```
mv docker-compose.yaml docker-compose.yaml.bck
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
##### ISI
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
```
enable global podman
```
```
sudo systemctl enable --global podman
```
### configure storage
```
nvim .config/{kelompok10/storage.conf
```
###### isi
```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```
```
nvim /etc/containers/registries.conf
```
```
unqualified-search-registries = ["docker.io"]
```
```
sudo chmod -R 777 app/slims
```
```
podman compose up -d
```
## bila berhasil runing, cek apakah slims kalian berhasil atau tidak

```
http://ip:8080
```

### cp 
https://github.com/slims/docker-compose-for-slims

https://wiki.archlinux.org/title/Podman
