# Dokumentasi Aplikasi Slims

---
## Download Podman-Compose
```
sudo pacman -S podman podman-compose
```

## Buat Directory
```
mkdir -p .config/containers
```
> Cek Directory Tersebut
```
ls .config
```
> Cek listall

```
ls -la
```

## Masuk Ke Dalam Directory Tersebut
```
cd .config/containers
```

## Install Slims Menggunakan Wget
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

## Unzip Slims
```
bsdtar -xzf master.zip
```

```
ls
```
Untuk mengecek kembali

```
rm -fr compose
```

## Move Directory
```
mv docker-compose-for-slims-master slims
```
```
ls
```
```
cd slims
```
Masuk directory slims

## Konfigurasi slims
```
nvim docker-compose.yaml 
```
> hapus bagian ipv4_address: 172.18.1.3 , ipv4_address: 172.18.1.4 , ipv4_addres: 172.18.1.11

> hapus simbol #

> ubah "127.0.0.1:6379:6379" menjadi "80:80"

> hapus config:

> hapus - subnet: "172.18.1.0/24"

## Membuat Hak Akses Dalam Folder
```
sudo chmod -R 777 app/slims
```

## Running Podman
```
podman compose up -d
```

## Konfigurasi Sysctl
```
nvim /etc/sysctl.d/99-custom.conf
```
Isi dengan 
```
net.ipv4.ip_unprivileged_port_start=80                                          
kernel.unprivileged_userns_clone=1
```
Cek
```
sysctl --system
```

## Konfig Containers
```
nvim ~/.config/containers/storage.conf 
```
Isi
```
[storage]                                                                       
driver = "overlay"                                                              
                                                                                
[storage/options.overlay]                                                       
mount_program = ""                                                              
mountopt = "userxattr"   
```

## Isi Registries
```
sudo nvim /etc/containers/registries.conf
```
cari baris ke-21 dan uncommenting serta masukkan dalam kurung ["docker.io, quai.io"]

## Cek Subuid Dan Subgid
```
nvim /etc/subuid
```
```
nvim /etc/subgid
```
> Isi Dengan [user]:100000:65536  

## Running Compose
```
podman compose up -d
```
Pull dulu jika belum bisa lalu podman compose up -d
```
podman pull docker.io/mysql:5.7
```
```
podman ps
```
