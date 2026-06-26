# Setup Podman

## 1. Konfigurasi SubUID dan SubGID

```bash
sudo nvim /etc/subuid
```
```
[user]:100000:65536
```

```
sudo nvim /etc/subgid
```
```
[user]:100000:65536
```
## 2. Aktifkan Podman

```bash
sudo systemctl enable --global podman
```

## 3. Buat Direktori Konfigurasi Container

```bash
mkdir -p ~/.config/containers
```

Edit file konfigurasi storage:

```bash
nvim ~/.config/containers/storage.conf
```

Isi file:

```ini
[storage]

driver = "overlay"

[storage.options.overlay]

mount_program = ""

mountopt = "userxattr"
```

## 4. Konfigurasi Registry Container

```bash
sudo nvim /etc/containers/registries.conf
```

Cari dan sesuaikan:

```ini
# unqualified-search-registries = ["docker.io"]
```

---

# Install SLiMS Container

## 1. Masuk ke Direktori Container

```bash
cd ~/.config/containers
```

## 2. Install Podman Compose

```bash
sudo pacman -S podman-compose
```

## 3. Download Source SLiMS

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

## 4. Ekstrak File ZIP

```bash
sudo pacman -S unzip
unzip master.zip
```

## 5. Verifikasi File

```bash
ls
```

## 6. Konfigurasi Port Unprivileged

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi file:

```ini
net.ipv4.ip_unprivileged_port_start=80
```

Terapkan konfigurasi:

```bash
sudo sysctl --system
```

## 7. Masuk ke Direktori Compose

```bash
cd compose
ls
```

## 8. Backup File Compose

```bash
mv docker-compose.yaml docker-compose.yaml.bck
ls
```

## 9. Edit File Compose

```bash
nvim docker-compose.yaml
```


### isi
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
    redis:
        image: redis:latest
        restart: always
        networks: 
            slims-net:
                ipv4_address: 172.18.1.4
        container_name: redis
        #ports:
        #    - "6379:6379"
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
            - "./conf/php/php.ini:/usr/local/etc/php/conf.d/php.ini"
networks: 
    slims-net:
        name: slims-net
        ipam:
            driver: default
            config:
              - subnet: "172.18.1.0/24"
```
### Jalankan 

```
sudo chmod -R 777 app/slims
```
### cek browser
```
http://ip:8080
```
```
sudo firewall-cmd --zone=public --add-
```
```
port=80/tcp --permanent
```
```
sudo firewall-cmd --reload
```
