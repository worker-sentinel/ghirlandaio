
install packet ```docker docker-composer git```

aktifkan dan jalankan service docker


ambil paket github slims resmi

```
sudo git clone https://github.com/slims/slims9_bulian.git slims9_bulian
```

lalu di ```.env``` tambahkan 
```
HTTP_PORT=8881
DB_PORT=3307
DB_ROOT_PASSWORD=ketikpasswordroot
DB_NAME=senayan
DB_USER=slimsuser
DB_PASSWORD=ketikpasswordDB
```

untuk ```docker-compose.yml``` isi

```
version: "3"

services:
  slims:
    container_name: slims
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - ENV=development
      - DB_HOST=db
      - DB_PORT=3306
      - DB_USER=${DB_USER}
      - DB_PASS=${DB_PASSWORD}
      - DB_NAME=${DB_NAME}
    volumes:
      - appfiles:/var/www/html/files
      - appimages:/var/www/html/images
      - apprepo:/var/www/html/repository
    ports:
      - "${HTTP_PORT}:80"
    networks:
      - slimsnet
    depends_on:
      - db
    restart: unless-stopped

  db:
    image: mariadb:lts
    container_name: db
    volumes:
      - ./install/senayan_ddl.sql:/docker-entrypoint-initdb.d/init.sql
      - dbdata:/var/lib/mysql
    environment:
      - MARIADB_ROOT_PASSWORD=${DB_ROOT_PASSWORD}
      - MARIADB_DATABASE=${DB_NAME}
      - MARIADB_USER=${DB_USER}
      - MARIADB_PASSWORD=${DB_PASSWORD}
    networks:
      - slimsnet
    restart: unless-stopped

networks:
  slimsnet:
    driver: bridge

volumes:
  dbdata:
  appfiles:
  appimages:
  apprepo:
```

cek

```
sudo docker compose config
```

jalankan container

```
docker compose up -d --build
```

Tes dari server

curl -I http://localhost:8881

```
sudo docker compose down -v
```
```
sudo docker compose up -d --build
```
```
sudo docker exec -u root slims sh -lc 'chown -R www-data:www-data /var/www/html/config && chmod -R 775 /var/www/html/config'
```
```
sudo docker composer restart
```
