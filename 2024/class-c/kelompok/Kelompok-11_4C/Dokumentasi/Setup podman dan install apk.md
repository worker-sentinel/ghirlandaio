## Setup podman dan install apk

# server1

## Install podman compose
```
sudo pacman -S podman-compose
```

```
rm -fr atom
```
```
mkdir container
```
```
cd container/
```
```
nvim storage.conf
```
```
sudo systemctl enable –global podman
```
```
sudo nvim /etc/containers/registries.conf
```
```
sudo nvim /etc/sysctl.d/[custome].conf
```
**setelah masuk ke tampilan itu kosong pencet “I” lalu ketik :**

```
kernel.unprevillaged_userns_clone = 1
```
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

```
sudo sysctl –system
```

**Bisa ke browser ketik atom compose 2.9 buat docker compose yang commandnya:**

```
git clone -b qa/2.X https://github.com/artefactual/atom.git atom
```
```
cd atom
```

**mengecek file yang sudah didownload tadi dengan menggunakan command**

```
ls
ls atom/
```
```
cd atom
```
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"
```
```
podman compose -f docker /docker-compose.dev.yml up -d
```
```
podman ps -a
```
*buat ngecek atom nya ada isinya atau engga*

## Install overlay
```
sudo pacman -S fuse-overlayfs
```
```
nvim docker/docker-compose.dev.yml
```
## Cek filesystem

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

# admin
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
