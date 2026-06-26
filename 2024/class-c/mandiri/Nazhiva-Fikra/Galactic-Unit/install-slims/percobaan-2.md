# Mendownload Git
```
sudo pacman -S git
```

# Menginstall Package Manager Phyton
```
pip install git+https://github.com/containers/podman-compose.git --break-systemp-packages
```
Gunanya untuk melindungi sistem dari konflik ketergantungan

# Membuat dan Memulai semua service dalam file compose dengan satu perintah
```
podman-compose up -d
```

# Mengecek kembali isi folder secara keseluruhan
```
ls -la
```

# Mengunduh file Slims 
```
mkdir slims
```
```
cd slims
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

# Membuka/mengekstra isi file yang sebelumnya sudah diunduh
```
unzip master.zip
```

```
mv docker-compose-for-slims-master compose
```
```
cd compose
```
```
sudo nvim /etc/sysctl.d/99-slims.conf
```
Tambahkan konfigurasi
```
net.ipv4.ip_unprivileged_port_start=80
```

# Menjalankan konfigurasi kernel semua direktori bersamaan
```
sudo sysctl --system
```
```
cat /etc/containers/registries.conf
```

# Mendownload MYSQL 5.7 menggunakan podman
```
podman pull docker.io/mysql:5.7
```
```
podman pull docker.io/redis:latest
```
```
podman pull docker.io/slimsofficial/slims:lates
```
```
podman compose up -d
```

# Mengecek kembali container, firewall dan ip
```
podman ps -a
```
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
ip a
```

# Keluar sesi
```
exit
```




