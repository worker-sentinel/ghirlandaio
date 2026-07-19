# Install Podman Compose

```bash
sudo pacman -S podman-compose
```

# Membuat Direktori Konfigurasi

```bash
mkdir -p ~/.config/kelompok7
```

# Masuk ke Direktori

```bash
cd ~/.config/kelompok7
```

# Mengunduh Source Code SLiMS

```bash
sudo pacman -S wget
```

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

# Ekstrak File ZIP

```bash
unzip master.zip
```

# Ubah Nama Folder

```bash
mv docker-compose-for-slims-master compose
```

# Pindah ke Folder Compose

```bash
cd compose
```

# Membuat Konfigurasi Kernel

```bash
sudo nvim /etc/sysctl.d/13-custom.conf
```

# Terapkan Konfigurasi Kernel

```bash
sudo sysctl --system
```

# Backup File `docker-compose.yaml`

```bash
mv docker-compose.yaml docker-compose.yaml.bck
```

# Gunakan Konfigurasi Redis

```bash
mv docker-compose-redis.yaml docker-compose.yaml
```

# Edit File `docker-compose.yaml`

```bash
nvim docker-compose.yaml
```

Lalu isi dengan konfigurasi berikut:

```yaml
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
```
