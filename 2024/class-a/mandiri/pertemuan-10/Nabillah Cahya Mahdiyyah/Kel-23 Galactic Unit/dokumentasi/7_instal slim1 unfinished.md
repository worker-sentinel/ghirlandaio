# Instalasi SLiMS Menggunakan Podman Compose

## 1. Instalasi Podman Compose

Install Podman Compose terlebih dahulu:

```
sudo pacman -S podman-compose
```

## 2. Instalasi Utilitas Pendukung

Install `wget` dan `unzip` jika belum tersedia:

```
sudo pacman -S wget unzip
```

## 3. Membuat Direktori Kerja

```
mkdir -p ~/.config/galactic
cd ~/.config/galactic
```

## 4. Mengunduh Source Docker Compose SLiMS

```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

## 5. Mengekstrak File dan Menyiapkan Direktori

```
unzip master.zip
mv docker-compose-for-slims-master compose
cd compose
```

## 6. Konfigurasi Port Non-Privileged

Buat atau edit file konfigurasi sysctl:

```
sudo nvim /etc/sysctl.d/13-custom.conf
```

Tambahkan baris berikut:

```
net.ipv4.ip_unprivileged_port_start=80
```

Simpan file, kemudian terapkan konfigurasi:

```
sudo sysctl --system
```

## 7. Mengganti File Docker Compose

Backup file konfigurasi lama dan gunakan konfigurasi Redis:

```
mv docker-compose.yaml docker-compose.yaml.bck
mv docker-compose-redis.yaml docker-compose.yaml
```

## 8. Konfigurasi Registri Container

Edit file registri Podman:

```
sudo nvim /etc/containers/registries.conf
```

Cari baris berikut:

```
# unqualified-search-registries = ["example.com"]
```

Ubah menjadi:

```
unqualified-search-registries = ["docker.io"]
```

wq kemudian keluar dari editor.

## 9. Mengaktifkan Layanan Podman

```
sudo systemctl enable --global podman.socket
sudo systemctl start podman.socket
```

Verifikasi status layanan:

```
systemctl --user status podman.socket
```

## 10. Menjalankan Container SLiMS

Pastikan berada pada direktori `compose`:

```
cd ~/.config/galactic/compose
```

Jalankan container:

```
podman compose up -d
```

## 11. Memeriksa Container yang Berjalan

```
podman ps
```

Untuk melihat log container:

```
podman compose logs -f
```

## 12. Mengakses Aplikasi SLiMS

Buka browser dan akses:

```
http://IP_SERVER:80
```

atau

```text
http://localhost
```

sesuai dengan konfigurasi jaringan yang digunakan.
