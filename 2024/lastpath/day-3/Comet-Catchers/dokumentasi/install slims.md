# Dokumentasi Instalasi SLIMS

## Download Source SLiMs
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip 
unzip master.zip
```
> ​wget -c ...: Mengunduh file ZIP konfigurasi Docker Compose untuk SLiMS dari GitHub.
> -c (continue) memastikan jika download terputus, prosesnya bisa dilanjutkan lagi.
> unzip master.zip: Mengekstrak file ZIP yang sudah di-download.

## Rename folder
```
mv docker-compose-for-slims-master compose
cd compose
```
> ​mv ... compose: Mengubah nama folder hasil ekstrak yang panjang menjadi lebih simpel, yaitu compose.
> ​cd compose: Masuk ke dalam folder tersebut untuk melanjutkan proses setup.

## Kernel
```
sudo nvim /etc/sysctl.d/99-custom.conf   
sudo chmod -R 777 app/slims  
sudo nvim /etcsubid   
```

Masukkan 
```
[user]:100000:65536      
sudo nvim /etcsugid   

dan

[user]:100000:65536
```

## configure storage
```
nvim ~/.config/cleo/storage.conf

[storage]  
driver = "overlay"  
[storage.options.overlay]                                                                                                                                                                 mount_program = ""   
mountopt = "userxattr"

sudo nvim /etc/containers
```

```
podman pull docker.io/mysql:5.7

podman compose up -d

podman ps -a

exit
```
