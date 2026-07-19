# INSTALL SLIMS cluster DATA

## COMMAND

### install podman compose
Menginstall podman compose untuk menjalankan dan mengelola banyak container bisa menggunakan command
```
Sudo pacman -S podman -compose
```

# Membuat Direktori Konfigurasi
Untuk membuat folder baru di dalam folder config bisa menggunakan command
```
Mkdir -p .config/{kelompok7} 
```

# Masuk ke Direktori
Pindah ke folder (kelompok 7) yang baru saja dibuat dengan menggunakan command
```
Cd .config/ {kelompok7}
```

#Mengunduh Source Code Slims
Mengunduh wget yang merupakan alat yang digunakan untuk mengunduh file dari internet melalui internal bisa menggunakan command
```
Pacman -S wget
```
```
Wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

# ekstrak file zip
Mengestrak file yang baru saja diunduh agar folder dan file didalamnya bisa diakses dengan menggunakan command
```
Unzip master.zip 
```

# ubah nama folder
Mengubah nama folder hasil ekstrak yang Panjang menjadi lebih pendek dengan menggiunakan command
```
Mv docker-compose-for-slims-master compose
```

# pindah ke folder compose
Masuk ke dalam folder compose untuk mulai melakukan configurasi cluster
```
Cd compose
```

#membuat konfigurasi kernel
Membuka text editor dengan hak akses administrator bisa menggunakan command
```
Sudo nvim /etc/sysctl.d/13-custom.conf
```

# terapkan konfigurasi kernel
Memberi tau system linux untuk langsung menerapkan konfigurasi kernel yang baru saja ditulis dengan menggunakan command
```
Sudo sysctl –system
```

## Backup file docker-compose
Mengubah nama file konfigurasi bawaan menjadi file Cadangan
```
Mv docker-compose.yaml docker-compose.yaml.bck
```

# gunakan konfigurasi redis
Mengaktifkan fitur redis dengan menggunakan command
```
Mv docker-compose-redis.yaml docker-compose.yaml
```

#edit file docker-compose
Membuka file konfigurasi utama menggunakan command
```
Nvim docker-compose.yaml
```

Lalu diisi
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
    #  - "3306:3306"
