# Install SLIMS

```
pacman -S podman-compose
```

```
mkdir -p .config/containers
```
Buat direktori konfigurasi containers.

```
cd .config/containers
```
Masuk ke direktori konfigurasi containers.

```
pacman -S wget
```
Install wget untuk download file dari internet.

```
unzip master.zip
```
Ekstrak file ZIP berisi docker-compose SLIMS.

```
mv docker-compose-for-slims-master compose
```
Rename folder hasil ekstrak menjadi compose.

```
cd compose
```
Masuk ke folder compose.

```
nvim /etc/sysctl.d/99-costum.conf
```
Buat file konfigurasi kernel. Tambahkan:

```
net.ipv4.ip_unprivileged_port_start=80
```
Agar user non-root bisa menggunakan port 80.

```
sysctl --system
```
Terapkan konfigurasi sysctl tanpa reboot.

```
mv docker-compose.yaml docker-compose.yaml.bck
```
Backup file docker-compose asli.

```
mv docker-compose-redis.yaml docker-compose.yaml
```
Gunakan versi docker-compose yang sudah include Redis.

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

```
chmod -R 777 app/slims .
```
Beri izin akses penuh ke folder app/slims agar container bisa baca dan tulis file.

```
podman compose up -d
```

```
ip a
```
Cek IP address server yang aktif.

```
podman compose up -d
```
Jalankan ulang container untuk memastikan semua service berjalan.

```
nvim /etc/subuid
```
Edit file subUID untuk user. Isi dengan:

```
[user]:100000:65536
```

```
nvim /etc/subgid
```
Edit file subGID untuk user. Isi dengan:

```
[user]:100000:65536
```

```
nvim ~/.config/user/storage.conf
```
Konfigurasi storage driver Podman untuk user. Isi dengan:

```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

```
nvim ~/.config/kel10/storage.conf
```
Konfigurasi storage driver Podman untuk user kel10. Isi dengan:

```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

```
mkdir -p ~/.config/user
```
Buat direktori konfigurasi user jika belum ada.

```
touch ~/.config/user/storage.conf
```
Buat file storage.conf kosong sebelum diisi.

```
nvim /etc/subuid
```
Tambahkan mapping untuk root. Isi dengan:

```
root:100000:65536
```

```
nvim /etc/subgid
```
Tambahkan mapping GID untuk root. Isi dengan:

```
root:100000:65536
```

```
podman compose up -d
```
Jalankan ulang semua container setelah konfigurasi diperbaiki.

```
podman ps -a
```
Cek status semua container termasuk yang berhenti.

```
podman pull docker.io/mysql:5.7
```
Download image MySQL 5.7 secara manual jika gagal otomatis.

```
podman compose up -d
```
Jalankan ulang container setelah image berhasil diunduh.

```
cd ..
```
