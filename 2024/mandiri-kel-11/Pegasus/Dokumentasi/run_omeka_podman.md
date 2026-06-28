


```bash
sudo systemctl enable --global podman
```



```bash
cd /etc/containers
```

ketik  dan cari 

```bash
nvim /etc/cintainers/registries.conf

```
ubah yg unculavi

![[IMG_20260624_194829_293.jpg]]


```bash
nvim /etc/sysctl.d/set.conf
```
ketik 
![[IMG_20260624_195222_413.jpg]]


```bash
sudo sysctl --system
```

Instalasi compose 

```bash
pacaman -S podman-compose
```

### bikin folder buat aplikasi

```bash
mkdir "nama"
```

```bash
cd "nama"
```

```bash
mkdir omeka
```

```bash
cd omeka
```

```bash
mkdir config
```

```bash
mkdir files
```

### ganti hak akses (jika file tak perlu -R)
```bash
chmod -R 777 config 
chmod -R 777 files
```

```bash
cd config
```

```bash
nvim database.ini
```
![[IMG_20260624_201932_723.jpg]]


buat file
```bash
nvim docker-compose.yml
```

Buat File Compose

Buat file:

```bash
nvim docker-compose.yml
```

Isi dengan:

```yaml
version: '2'

services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: omeka
      MYSQL_DATABASE: omeka
      MYSQL_USER: omeka
      MYSQL_PASSWORD: omeka

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

Jalankan Container

```bash
sudo podman compose up -d
```

 Verifikasi Container

```bash
sudo podman ps
```

Akses Omeka S

Buka browser:

```text
http://IP_SERVER
```


umount -R  container

lvresize -L /desa

