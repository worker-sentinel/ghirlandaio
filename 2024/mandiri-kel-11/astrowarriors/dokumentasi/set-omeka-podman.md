# Set Omeka Podman

```
sudo systemctl enable --global podman
```

```
systemctl status podman
```

```
sudo su
```

buat direktori
```
mkdir [terserah]
```

```
cd [terserah]/
```

buat direktori omeka
```
mkdir omeka
```

```
cd omeka/
```

buat direktori config
```
mkdir config
```

buat direktori files
```
mkdir files
```

buat hak akses direktori
```
chmod 777 config/
chmod 777 files/
```

```
ls -la
```

```
cd config/
```

buat file
```
nvim database.ini
```
isi
```
user = "root"
password = "buat-sendiri"
dbname = "omeka"
host = "sesusaikan-dengan-komputer"
port = "3306"
```

```
cd ..
```

buat file
```
nvim docker-compose.yml
```
isi
```
services:
  omeka:
    image: elestio/omeka:latest
    container_name: omeka-app
    restart: always
    ports:
      -"sesuaikan-dengan-ip-a:8080:80"
    network_mode: "host"
    environment:
      -APACHE_SERVER_NAME=localhost
    volumes:
        - ./files:/var/www/html/omeka-s/files
        - ./config/database.ini:/var/www/html/omeka-s/config/database.ini
    depends_on:
      - mariadb

  mariadb:
    image: mariadb:latest
    container_name: database-omeka
    ports:
      - "3306"
    network_mode: "host"
    environment:
      - MYSQL_ROOT_PASSWORD=buat-sendiri
    volumes:
      - mariadb_data:/var/lib/mysql

volumes:
  mariadb_data:
```

pull image
```
podman compose up -d
```

# After Pulling Image

samakan wifi server 1 dengan client

```
iwctl
```

masuk ke direktori yang berisi direktori omeka

masuk ke direktori omeka
```
cd omeka/
```

```
sudo su
```

aktifkan compose
```
podman compose up -d
```

cek status compose
```
podman ps -a
```

cek IP podman
```
ip a
```

jika compose sudah aktif, bisa cek di browser di device client
```
http://[ip podman]:[port omeka]
```

contoh jika telah berhasil muncul di browser
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/WhatsApp%20Image%202026-06-25%20at%2013.57.03.jpeg)
