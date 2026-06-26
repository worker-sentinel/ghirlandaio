## SET IP SERVER 1

Buka file
```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Ubah file, sesuaikan dengan foto
> catatan : 
Angka ip bebas, asal gak lebih dari 255.255.255

IP ADDRESS KITA :
Address = 199.19.9.8/24
Getaway = 199.19.9.1

Ip static server 1 selesai!!


## SET IP STATIC SERVER 2 

ssh admin dan server 2, dari terminal admin 
```bash
ssh username2@ipserver
```

Kalo error (ssh ga mau connect dengan admin dan server):
Buka tab baru di terminal admin
Pastikan satu koneksi dengan admin, server 1 dan server 2
Coba matikan firewalld di server 2 dengan command:

```bash
Sudo systemctl stop firewalld
```

Set up ip server2
```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

IP ADDRESS KITA :
Address = 199.19.9.8/24
Getaway = 199.19.9.1
Dalam kondisi kabel LAN server 1 dan 2 masih terhubung lakukan reset 

### reset server 1
```bash
Sudo systemctl restart systemd-networkd
```

### Server 2 direset
```bash
Sudo systemctl restart systemd-networkd
```

Cek apakah ip sudah muncul di server 1 dan 2
```bash
ip a
```



(bikin dokumentasi baru buat NGINX)

# INSTALL NGINX DI SERVER 2

Server 2 kita akan bikin proxy 

ssh ulang admin ke server 2 
```bash
ssh nama@ipserver2
```

Install nginx 
```bash
sudo pacman -S nginx
```

Aktifkan nginx
```bash
sudo mkdir -p /etc/nginx/sites-available
```

```bash
Sudo mkdir -p /etc/nginx/sites-enabled
```

Buka file konfigurasi nginx
```bash
sudo nvim /etc/nginx/nginx.conf
```
Lalu edit : 
Hapus tagar di user http
Masukkan di akhir https server `include /etc/nginx/sites-enabled/*`

lalu simpan `esc` dan `:wq`

```bash
sudo nvim /etc/nginx/sites-available/slims.conf
```

Di sini jangan sampai typo, harus hati-hati 
```bash
server {
       listen 80;
       server_name slims.test;

       location / {
              proxy_pass http://ipstaticerver1:80; 
	  proxy_set_header Host $host;
	  proxy_set_header X-Real-IP $remote_addr;
	  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	  proxy_set_header X-Forwarded-Proto $scheme;
       }
}


Symlink kaya nge copy file slims.conf ke dalam sites-enabled

```bash
sudo ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled
```

Lihat apakah sudah ada file nya 
```bash
Sudo nginx -t
```

Restart nginx
```bash
Sudo systemctl restart nginx
```
## BUTUH TERMINAL ADMIN

```bash
sudo nvim /etc/hosts
```
Enter 2 kali dibawah ::1
Enter sekali 
Masukkan ip server 2 yang wlan0 lalu tambahkan slims.test 

Esc
:wq

Cek di firefox di laptop admin `http://slims.test/slims/install/index.php`


Kalau error, cek containers: 

```bash
Sudo podman ps -a
```

Kalau status nya tidak up, maka aktifkan dulu
```bash
cd docker-compose-for-slims-master
```

```bash
sudo podman compose down
```

```bash
sudo podman compose up -d
```

Di server 2
```bash
rm -fr /etc/nginx
```

```bash
sudo ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
```

```bash 
sudo nginx -t
```

```bash
sudo nvim /etc/nginx/nginx.conf
```
