# Menjalankan Omeka di Podman

Install podman desktop
```
sudo pacman -S podman-compose
```

Buat directory untuk file
```
mkdir -p ./files
```

Tetapkan izin untuk akses directory
```
chmod -R 777 ./files
```

Buat file untuk konfigurasi database
```
nvim ./config/database.ini
```

Tambahakan ke file ./config/database.ini
```
user = "root"
password = "buat-sendiri"
dbname = "omeka"
host = "sesusaikan-dengan-komputer"
port = "3306"
```

Buat file compose untuk container
```
nvim compose.yaml
```

Buat container omeka dan container database mariadb di file compose.yaml
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

Cek container yang telah dibuat
```
podman ps -a
```

Buat database untuk container
```
podman exec -it database-omeka mariadb -u root -pbuat-sendiri

CREATE DATABASE omeka;

exit
```

Aktifkan podman compose
```
podman compose up -d
```

Cek omeka di browser
```
http://localhost:8080
```

Sumber: https://hub.docker.com/r/elestio/omeka
