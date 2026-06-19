1. Instalasi Podman Compose
```
sudo pacman -S podman-compose
```

2. Membuat Direktori
```
mkdir -p .config/galactic  
cd .config/galactic
```

3. Mengunduh Source Docker Compose SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
4. Instal Wget jika belum ada 
```
sudo pacman -S wget unzip
```

5. Mengekstrak File dan Menyiapkan Direktori
```
unzip master.zip  
mv docker-compose-for-slims-master compose
 cd compose
```

6. Konfigurasi Port Non-Privileged
```
sudo nvim /etc/sysctl.d/13-custom.conf
```
tambahkan konfigurasi
```
net.ipv4.ip_unprivileged_port_start=80
sudo sysctl –system
```

7. Mengganti File Docker Compose
```
mv docker-compose.yaml docker-compose.yaml.bck                                                                                                                      mv docker-compose-redis.yaml docker-compose.yaml                                                                                                                    
```


8. Mengaktifkan Layanan Podman
```
sudo systemctl enable --global podman    
ls
cd compose
```

9. Menjalankan Container SLiMS
```
podman compose up -d
```

10. Konfigurasi Registri Container
```
sudo nvim /etc/containers/registries.conf
```
merubah
#  # An array of host[:port] registries to try when pulling an unqualified image, in order.                                                                                                    # unqualified-search-registries = ["example.com"]
ganti dengan
# An array of host[:port] registries to try when pulling an unqualified image, in order.                                                                                                      # unqualified-search-registries = ["dockeri.io"] 
    
11. Mengaktifkan dan Memeriksa Status Podman
```
sudo systemctl enable --global podman                                                                                                                              sudo systemctl status podman
sudo systemctl enable podman
```



