# Menginstall slims

## Cek alamat ip
```
ip a
```
## cek status ssh
```
systemctl status sshd 
```
## start ssh
```
systemctl start sshd
```
## connect ssh dari komputer lain
```
ssh sagi@192.168.1.6
```
## masukan password user
-
## verifikasi ssh
```
systemctl status sshd
```
kalau sudah active tulisannya 
_active: active (running)_
## cek versi kernel 
```
uname -a
```
## masuk ke direktori
```
cd /etc/mkinitcpio.d
```
## mengedit preset kernel
```
sudo sudo nvim linux-hardened.preset
```
## membuat ulang initrams
```
sudo mkinitspio -P
```
## aktifkan podman
```
sudo systemctl enable --global podman 
```
## kembali ke direktori home
```
cd ~
```
## registries podman
```
sudo nvim /etc/containers/registries.conf
```
## selanjutnya 
```
i
```
. # unqualified-search-registries = ["example.com"]
lalu hapus # dan ganti examplenya ke docker.io
unqualified-search-registries = ["docker.io"]  
lalu 
```
:wq
```
## konfigurasi sysctl
```
sudo nvim /etc/sysctl.d/custome.conf
```
lalu 
```
i
```
isi
```
kernel.unprivileged_userns_clone=1
```
```
:wq
```
## menginstall podman compose
```
sudo pacman -S podman-compose
```
## cek status networkmanager
```
sudo systemctl status NetworkManager
```
## cek status iwd
```
systemctl status iwd
```
## cek status networkmanager
```
systemctl status NetworkManager
```
## download docker compose slims
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip   
```
## install unzip
```
sudo pacman -S unzip
```
## masuk direktori docker compose slims
```
cd docker-compose-for-slims  
```
```
unzip master.zip
```
## mengubah hak akses
```
sudo chmod -R 777 app/slims
```
## menjalankan slims
```
sudo podman compose up -d
```

<img width="1600" height="900" alt="WhatsApp Image 2026-06-26 at 5 09 16 AM" src="https://github.com/user-attachments/assets/2dca1182-cd7b-4341-a8eb-ae7d15c44ebd" />
<img width="1280" height="963" alt="WhatsApp Image 2026-06-26 at 5 09 16 AM(1)" src="https://github.com/user-attachments/assets/858b84df-db0a-4442-a211-45b0b8f7b2c7" />

