## Masuk ke Direktori Kontainer Pengguna

```bash
mkdir -p ~/.config/containers/
cd ~/.config/containers/
```

## Instalasi Tools Pendukung

```bash
pacman -S --noconfirm wget unzip
```

## Mengunduh Source Code SLiMS

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

## Ekstraksi dan Penataan Direktori

```bash
unzip master.zip

mv docker-compose-for-slims-master compose

cd compose
```

## Optimasi Sistem Kernel 

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

```bash
sudo sysctl --system
```

## Konfigurasi Berkas Compose 

```bash
mv docker-compose.yaml docker-compose.yaml.bck
```

```bash
mv docker-compose-redis.yaml docker-compose.yaml
```

## Pengaturan Hak Akses Direktori

```bash
sudo chmod -R 777 app/slims
```

## Menjalankan Kontainer SLiMS

```bash
podman compose up -d
```

## Konfigurasi Jaringan & Pembukaan Port 80

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

```bash
firewall-cmd --reload
```
