# Binding

Di tahap ini setiap server akan dilakukan binding menggunakan google kubernetes engine.

## 1. Download k3s 

```bash
sudo curl -sfL https://get.k3s.io | sh -s server
```

Setelah berhasil diunduh, lakukan pengecekan status k3s

```bash
sudo systemctl status k3s.service
```

## 2. Buat Port 

Masuk ke root

```bash 
sudo su
```

Masuk ke direktori /var/lib 

```bash 
cd /var/lib
```

Cek file yang tersedia

```bash
ls 
```

Dapatkan ip address, pastikan menggunakan koneksi yang sama

```bash
Ip a
```



Cek status Firewall

```bash
  sudo systemctl status firewalld
```

Tambahkan port di zona publik

```bash
Sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```

Lakukan reload firewall

```bash
sudo firewall-cmd --reload
```

Membuat atau mendapatkan token

```bash
sudo kubectl get node
```

```bash
sudo su
```

```bash
cd /var/lib/rancher/k3s/server/
```

Pastikan node ada

```bash
ls
```

Dapatkan node

```bash
cat node-token
```

## Di Admin

```bash 
ssh nama@ip
```

## 1. Menginstal k3s

```bash
sudo curl -sfL https://get.k3s.io | K3S_URL=”https://ip.:6443” K3S_TOKEN=”token” sh -s - agent 
```

Cek status

```bash 
sudo systemctl status k3s-agent.service
```



Jika tidak aktif, lakukan restart

```bash 
Sudo systemctl restart k3s-agent.service
```

Lalu cek lagi

```bash
sudo systemctl status k3s-agent.service
```

# SLIMS

## SSH dari Kontrol

Terus ssh di admin 

```bash
Sudo systemctl status sshd
```
Jika sudah aktif maka ok, terus lanjut 

Aktifkan podman
```bash
Sudo systemctl enable --global podman
```

Masukkan docker.io ke dalam file podman
Bash```
sudo nvim /etc/containers/registries.conf
```

Pergi ke line 21, masuk ke mode insert dan hapus simbol tagar (#), diganti jadi `[“docker.io”]`
Keluar dari mode insert, `esc` dan simpan `:wq`

Pergi ke file, akses username dari kernell hardened
```bash
sudo nvim /etc/sysctl.d/custom.conf
```

Masukkan akses username space di Linux-hardened, masuk mode insert
```bash
kernel.unprivileged_userns_clone=1
```

Keluar dari mode insert `esc` dan simpan `:wq`

Instalasi aplikasi 
```bash
sudo pacman -S podman-compose
```

Install docker compose untuk slims

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

Kalau ada masalah dengan wget, maka install wget dulu 
```bash
Pacman -S wget
```

Lalu coba install ulang docker compose untuk slims.

Setelah itu unzip file, kalau belum ada install unzip dulu `pacman -S unzip`

```bash
unzip master.zip
```

Cek hasil unzip
```bash
Ls
```

Dari unzip akan tercipta folder `docker-compose-for-slims-master` (folder ini penting).


Masuk ke directory docker compose
```bash
cd docker-compose-for-slims-master
```

cek apakah sudah masuk ke compose
```bash
ls
```

Ganti izin app
```bash
sudo chmod -R 777 app/slims
```

Jalankan isi dari file compose
```bash
sudo podman compose up -d
```

Jika error karena jam maka jalankan command 
```bash
```


Mengecek apakah containers slims sudah berjalan
```bash
sudo podman ps -a 
```

Hentikan firewalld
```bash
Sudo systemctl stop firewalld
```

Buka ip server
```bash
ip a 
```

Buka browser lalu ketik `http://ipserver:80` untuk memunculkan tampilam installer slims. Apabila sudah ada, maka containers sudah running dan slims sudah tersedia.

<img width="1599" height="999" alt="image" src="https://github.com/user-attachments/assets/10034f65-e577-435a-bcd9-54eca063dc8a" />
