# Setup podman dan install atom
## Install podman compose
```
sudo systemctl enable –global podman
```
```
sudo nano /etc/containers/registries.conf
```
setelah itu masukkan ini
```
# # An array of host[:port] registries to try when pulling an unqualified image, in order.
unqualified-search-registries = ["docker.io"]
```
```
sudo nano /etc/sysctl.d/custome.conf
```
setelah itu masukkan ini
```
kernel.unprivileged_userns_clone=1
```
```
sudo sysctl --system
```
```
sudo su
```
```
mkdir container
```
```
cd container
```
```
git clone -b qa/2.x https://github.com/artefactual/atom.git atom
```
```
ls
atom
```
```
cd atom
```
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"
```
```
podman compose -f docker/docker-compose.dev.yml up -d
```
## Install overlay
```
sudo pacman -S fuse-overlayfs
```
```
nvim docker/docker-compose.dev.yml
```
## Cek Filesystem
```
df -h
```
```
cat /etc/fstab
```
## Jalankan ulang container
```
podman compose -f docker/docker-compose.dev.yml up -d
```
```
masuk kebrowser, masukin http://[ip server:63001]
```
## Admin
```
ssh (username)@(ip server) 
```
## Status firewall
```
sudo systemctl status firewalld
```
## Liat service apa aja yang udah di daftarin sama firewall
```
sudo firewall-cmd –list-all-zone
```
