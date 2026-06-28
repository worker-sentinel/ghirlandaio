### *SERVER 2*
### Melihat IP address
```
 ip a
```

### *SERVER 2 DI LAPTOP ADMIN*
### Koneksi ke server
```
ssh unit@192.168.100.5
```

```
sudo nvim /etc/systemd/network/20-ethernet.network
```

### Bikin ip sendiri
```
[Match]
Type=ether

[Link]
RequiredForOnline=routable

[Network]
Address=23.23.2.4/24
Gateway=23.23.2.1
DNS=1.1.1.1 8.8.8.8
```
 
### Menginstall dan mengaktifkan Ngix
```
sudo pacman -S nginx
sudo systemctl enable --now nginx
sudo nvim /etc/nginx/nginx.conf
```
> lalu ketik di paling bawah sebelum } yg paling bawah 

```
include /etc/nginx/sites-enabled/*;
```
```
sudo mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
```

### Menerapkan konfigurasi jaringan
```
systemctl restart systemd-networkd
```
```
sudo nvim /etc/nginx/sites-available/slims.conf
```
> isi ini:
```
server {
    listen 80;
    server_name slims.test;

    location / {
        proxy_pass http://192.168.100.78:80;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Aktifin ulang config slimsnya
```
sudo ln -s /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
```

```
sudo nginx -t
```
### Restart Nginx
```
sudo systemctl restart nginx
```
