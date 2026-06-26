# kita enable terlebih dahulu podman-nya secara global
```
sudo systemctl enbale --global podman
```
# lalu kita masuk ke dalam directory containernya
```
cd /etc/containers
```
# lalu masuk ke configuration
```
sudo nvim /etc/containers/registries.conf
```
### lalu cari ada tulisa "unqualified"

<img width="357" height="369" alt="image" src="https://github.com/user-attachments/assets/6e491ef1-fec2-42fe-9f0e-364fc061448a" />

>lalu bagian tersebut di hapus pagarnya dan "example.com" di ganti menajdi "docker.io"

## setelah itu buar file configuration
```
sudo nvim /etc/sysctl.d/[namafile.conf]
```
lalu isi
```
kernel.unprivileged_userns_clone=1
```
# lalu cek apakah file directorynya
```
sysctl --system
```
<img width="414" height="301" alt="image" src="https://github.com/user-attachments/assets/79114f63-f686-43f0-bc48-6aac7396511f" />

# install podman-compose nya
```
sudo pacman -S podman-compose
```
# kita buat directory composenya
```
mkdir nova
```
# masuk kedalam directorynya
```               
cd nova/
```
# setelah masuk ke dalam directorynya
kita buat folder untuk omeka
```
mkdir omeka
```
# lalu masuk ke dalam folder omeka
```
cd omeka/    
```
# lalu buat folder config dan files
```
mkdir config
```
```
mkdir files                                                                       
```
# lalu kita ls apakah sudah terbuat apa tidak
```
ls
```
# setelah itu kita beri akses
```
chmod -R 777 config/
chmod -R 777 files/
```
```
ls -la
```
# masuk ke dalam foled config
```
cd config/
```
# kita buat file databasenya
```
nvim database.ini
```
isi

<img width="71" height="34" alt="Screenshot 2026-06-25 202346" src="https://github.com/uattachments/assets/4ed1c874-067e-4fa0-9ffd-cd19c6117acc" />

# setelah di isi
keluar dari folde config
```
cd ..
```
# pastikan berapa di dalam folder omeka
#### setelah itu kita buat file docker-composenya
```
nvim docker-compose.yml
```

<img width="193" height="106" alt="Screenshot 2026-06-25 203603" src="https://github.com/user-attachments/assets/f81de28c-5316-4884-bc28-e1d7188ab6a5" />

# setelah dibuat running-kan docker-compose nya
```
podman compose up -d
```                                                                                                                                  
```


