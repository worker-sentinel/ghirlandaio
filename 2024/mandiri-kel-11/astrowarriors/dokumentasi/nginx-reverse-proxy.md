# Nginx Reverse Proxy

sebelumnya pastikan container compose sudah aktif di server 1 aplikasi. 

## server 2

```
sudo pacman -S nginx
```

```
sudo nvim /etc/nginx/nginx.conf
```
isi 
> tambahkan di sebelum `}` terakhir
```
include /etc/nginx/sites-enabled/*;
```

```
sudo mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
```

> sesuaikan `[terserah]` dengan nama yang ingin dibuat
```
sudo nvim /etc/nginx/sites-available/[terserah].conf
```
isi
> seseuaikan server_name [terserah] dengan yang domain yang ingin dibuat
> sesuaikan proxy_pass dengan ip static dan port omeka. 
```
server {
    listen 80;
    server_name [terserah];

    location / {
        proxy_pass http://15.15.15.5:8081;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```
sudo ln -s /etc/nginx/sites-available/asci.conf /etc/nginx/sites-enabled/
```

```
sudo nginx -t
```

```
sudo systemctl restart nginx
```

## client

```
sudo nvim /etc/hosts
```

> sesuaikan server ip dengan yang di server 2
> sesuaikan domain dengan server_name yanh sebelumnya telah dibuat
```
server_ip [terserah]
```

```
http://[terserah].com
```

contoh jika telah berhasil muncul di browser
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/IMG_20260626_105050.jpg)
