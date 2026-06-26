**install omeka menggunakan podman di server2**
#  SETUP OMEKA
```
sudo systemctl enable --global podman
```
```
systemctl status podman
```

```
sudo su
```
```
mkdir [apikasi]
```
```
cd [aplikasi]
```
```
mkdir omeka
```
```
cd omeka
```
```
mkdir config
```
```
mkdir files
```
```
cd ..
```
```
chmod -R 777 omeka
```
```
cd omeka
```
```
cd config/
```
```
nvim database.ini
```
> isi
```
user     = "omeka"
password = "omeka"
dbname   = "omeka"
host     = "db"
port     = "3306"
```

```
cd ..
```
```
nvim docker-compose.yml
```
> isi
```
version: '2'
services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL ROOT PASSWORD: omeka
      MYSQL DATABASE: omeka
      MYSQL USER: omeka
      MYSQL PASSWORD: omeka

omeka-s:
  depends_on:
    - db
  build: ./
  image: elestio/omeka:latest
  ports:
    - "8081:80"
  volumes:
    - ./files:/var/www/html/omeka-s/files
    - ./config/database.ini:/var/www/html/omeka-s/config/database.ini
  restart: always
```

## pull image
```
podman compose up -d
```

### akses di browser
```
http://ipserver:8081
