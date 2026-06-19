# Instal SLiMS

## 1. Cek IP Server

Di server:

```bash
ip a
```

Di laptop admin:

```bash
ping <ip-server>
```

---

## 2. Remote Server dengan SSH

```bash
ssh <username-server>@<ip-server>
```

Jika OpenSSH belum terpasang:

```bash
sudo pacman -S openssh
```

Aktifkan SSH:

```bash
sudo systemctl enable --now sshd
```

---

## 3. Install Podman Compose

```bash
sudo pacman -S podman-compose
```

Masukkan password jika diminta.

---

## 4. Membuat Direktori Kerja

```bash
mkdir -p ~/.config/celadon
cd ~/.config/celadon
```

Download repository:

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

Cek file:

```bash
ls
```

---

## 5. Install Unzip

```bash
sudo pacman -S unzip
```

Ekstrak file:

```bash
unzip master.zip
```

Ubah nama folder:

```bash
mv docker-compose-for-slims-master compose
```

Cek kembali:

```bash
ls
```

Masuk ke folder compose:

```bash
cd compose
```

---

## 6. Konfigurasi Sysctl

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi file:

```conf
net.ipv4.ip_unprivileged_port_start = 80
```

---

## 7. Konfigurasi Docker Compose

Backup file lama:

```bash
mv docker-compose.yaml docker-compose.yaml.bck
```

Gunakan konfigurasi Redis:

```bash
mv docker-compose-redis.yaml docker-compose.yaml
```

Kosongkan file:

```bash
echo '' > docker-compose.yaml
```

Cek isi file:

```bash
cat docker-compose.yaml
```

Edit file:

```bash
nvim docker-compose.yaml
```

Isi file:

```yaml
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

---

## 8. Ubah Permission Folder

```bash
sudo chmod -R 777 app/slims
```

---

## 9. Menjalankan Container

```bash
podman compose up -d
```

---

# Jika Terjadi Error Rootless Podman

Edit file:

```bash
sudo nvim /etc/subuid
```

Isi:

```text
[user]:100000:65536
```

Edit file:

```bash
sudo nvim /etc/subgid
```

Isi:

```text
[user]:100000:65536
```

---

## 10. Konfigurasi Storage

```bash
nvim ~/.config/celadon/storage.conf
```

Isi:

```conf
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

---

## 11. Konfigurasi Registries

```bash
sudo nvim /etc/containers/registries.conf
```

---

## 12. Jalankan Ulang Container

```bash
podman compose up -d
```

Cek status container:

```bash
podman ps -a
```

Status `Up` menandakan container berhasil berjalan.

---

## 13. Cek IP Server

```bash
ip a
```

Salin IP server.

Buka browser:

```text
http://<ip-server>
```

---

# Jika Belum Bisa Diakses

Buka port HTTP:

```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```

Reload firewall:

```bash
sudo firewall-cmd --reload
```

Cek konfigurasi firewall:

```bash
sudo firewall-cmd --info-zone=public
```

---

## 14. Akses SLiMS

Buka browser:

```text
http://<ip-server>:80
```
