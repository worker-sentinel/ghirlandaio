# Install Slims
## Login server melalui ssh
```
ssh sagitarius@192.168.1.30
```
>masukkan password
## Melihat container
```
podman ps
```
## Directori project
```
 cd .config/containers/docker-compose-for-slims-master/
```
## Melihat isi folder
```
ls
```
## Menjalankan container
```
podman compose up -d
```
## Menampilkan lokasi direktori
```
pwd
```
## Konfigurasi kernel
```
sudo sysctl --system
```
>masukkan password
## Mengedit file konfigurasi
```
sudo nvim /etc/sysctl.d/99-custom.conf
```
> Insert
```
net.ipv4.ip_unprivileged_port_start=80
```
## Memastikan podman berjalan
```
podman ps
```
## melihat Ip
```
ip a
```
## Membuka port 80 firewall 
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
## Memuat ulang firewall
```
sudo firewall-cmd --reload
```
## Logout
```
logout


exit
