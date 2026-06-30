# Deploy omeka pada podman
```
sudo pacman -S  podman-compose
```

# membuat file compose nya
```
mkdir omeka-project
```

```
nvim compose.yml
```

# config compose.yml
```
version: '3'

services:
  db:
    image: docker.io/library/mariadb:10.11
    container_name: omeka-db
    environment:
      MYSQL_ROOT_PASSWORD: kelompok11
      MYSQL_DATABASE: omeka
      MYSQL_USER: omeka
      MYSQL_PASSWORD: omeka  
    volumes:
      - db_data:/var/lib/mysql:Z
    restart: always

  omeka:
    image: docker.io/omeka/omeka-s:latest
    container_name: omeka-app
    ports:
      - "8080:80"
    volumes:
      - files_data:/var/www/html/files:Z
    depends_on:
      - db
    restart: always

volumes:
  db_data:
  files_data:
```
# aktifkan container 
```
podman-compose up -d
```
> tunggu sebentar selama 20 detik

# lalu restart

```
podman restart omeka-app
```

# cara menghapus container
```
podman ps -a
```
> untuk list container yang aktif

```
podman rm -rf [nama kontainer]
```

```
pdoman volume prune 
```
> untuk menghapus sisa sisa kontainer yang tidak terpakai
