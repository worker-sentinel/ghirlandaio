**di sini kami menggunakan server 2 sebagai reverse proxy dari aplikasi yang di deploy di server 1**
 ### di sisi server 2
```
sudo pacman -S nginx
```
```
sudo nvim /etc/nginx/nginx.conf
```
tambahkan ke paling bawah. sebelum tutup `}` terakhir
```
include /etc/nginx/sites-enabled/*;
```
```
sudo mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
```
```
sudo nvim /etc/nginx/sites-available/omeka.conf
```
isi dengan
```
server {
    listen 80;
    server_name agoy.local.test;

    location / {
        proxy_pass http://10.10.1.3:8081;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
```
sudo ln -s /etc/nginx/sites-available/omeka.conf /etc/nginx/sites-enabled/
```
```
sudo nginx -t
```
```
sudo systemctl restart nginx
```

### di sisi client

```
sudo nvim /etc/hosts
```
> tambahkan domain sesuai di nginx
> contoh
```
# Static table lookup for hostnames.
# See hosts(5) for details.
127.0.0.1        localhost
::1              localhost

10.10.1.4(sesuaikan dengan ip wireless server 2)      	 agoy.local.test

```
```
sudo systemctl restart nginx
```

> contoh akses browser:
```
http://agoy.local.test.com
```
