# Dokumentasi active podman pada server app
## Check apakah linux kalian sudah hardened atau masih lts
```
uname -a
```
**lanjut**
```
cd /etc/mkinitcpio.d
```
```
ls -la
```
**lalu kita hapus linux lts nya**
```
rm -fr linux-lts-preset
```
```
exit
```
```
reboot
```
```
uname -a
```
```
sudo systemctl enable –global podman
```
```
sudo nvim /etc/containers/registries.conf
```
**cari pada bagian unqualified, hapus hashtagnya/ uncommenting dan pada bagian example.com itu diganti menjadi “docker.io”**
```
:wq
```
```
sudo nvim /etc/sysctl.d/custom.conf
```
**isi dengan:**
```
kernel.unprivileged_userns_clone=1
```
```
:wq
```
```
sudo pacman -S podman-compose
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
**kalau gagal, berarti kita download dulu wget nya**
```
sudo pacman -S wget
```
**kita coba lagi wget -c nya**
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
unzip master.zip
```
```
ls
```
```
cd docker-compose-for-slims-master
```
```
ls
```
```
sudo chmod -R 777 app/slims
```
```
sudo podman compose up -d
```
```
sudo podman ps -a
```
```
sudo systemctl stop firewalld
```
ip a
```
```
exit
```
