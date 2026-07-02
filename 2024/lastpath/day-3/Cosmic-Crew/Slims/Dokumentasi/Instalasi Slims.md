# Instalasi Slims
```
Sudo pacman -S podman-compose
```

## Menyiapkan Directori Container
```
mkdir -p .config/containers
```
```
ls .config/
```
```
ls -la
```
```
cd .config/containers
```

## Install Slims
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
ls -la
```
```
cd ..
```
```
ls
```
```
ls -la
```

## Setup Hak Akses Direktori
```
sudo chown -R kel10:kel10 containers/
```
```
sudo chown -R kel10:wheel containers/
```
```
ls -la
```
```
sudo chown -R kel10:kel15 containers/
```
```
cd containers/
```
```
ls
```
```
ls -la
```

## Mengekstrak Source Slims
```
rm master.zip
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
bsdtar -xzf master.zip
```
```
ls
```
```
sudo rm -fr compose
```
```
ls
```
```
mv docker-compose-for-slims-master slims
```
```
ls
```
```
cd slims
```
```
ls
```

## Membuat Docker Compose 
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
        command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE_ERR
OR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION --max_allowed_packet=1024M
        volumes:
            - "./dbdata:/var/lib/mysql"
        ports:
          - "127.0.0.1:3306:3306"
    app01:
        image: slimsofficial/slims:latest
        restart: always
        networks:
            - slims-net
        container_name: slims-app
        ports:
            - "8080:80"
        #    - "443:443"
        volumes:
            - "./app:/var/www/html"
networks:
   slims-net:
       name: slims-net
       ipam:
           driver: default 
```

```
sudo chmod -R 777 app/slims
```

## Konfigurasi Sysctl
```
sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi dengan:
>net.ipv4.ip_unprivileged_port_start=80

>kernel.unprivileged_userns_clone=1

```
sudo sysctl --system
```

## Konfigurasi Storage Podman
```
nvim ~/.config/containers/storage.conf
```

ADD:
```
[storage]
driver = "overlay"

[storage/options.overlay]
mount_program = ""
mountopt = "userxattr"
```

## Konfigurasi Registry Podman
```
sudo nvim /etc/containers/registries.conf
```
>Pada baris 'unqualified-search', hapus spasi diawal dan "quay.io"

## Menjalankan Container Slims
```
podman compose up -d
```

## Troubleshooting Storage Podman
```
nvim ~/.config/containers/storage.conf
```
>Pada bagian [storage/options.overlay] ubah menjadi [storage.options.overlay]

## Troubleshooting Container
```
sudo su
```
```
ls
```
```
podman compose up -d 
```
```
ls /home/
```

```
nvim /etc/modprobe.d/01-custum.conf^C
```
```
exit
```

## Memindahkan Direktori Compose 
```
ls
```

Cek
```
nvim docker-compose.yaml
```

```
cd ..
```

```
mv slims/compose
```

```
cd compose/
```

## Menjalankan Container Kembali
Masuk sebagai root
```
sudo su
```

```
podman compose up -d
```

## Verifikasi Container
Melihat container yang sedang berjalan
```
podman ps
```

```
podman ps -a
```

## Selesai
```
exit
``` 

# Percobaan akses di server 1
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/c59adb2e-7834-4d8a-ad4f-42533acabff0" />
