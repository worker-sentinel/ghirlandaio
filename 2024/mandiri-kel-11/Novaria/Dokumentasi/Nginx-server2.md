# HOW TO INSTALL NGINX FOR SERVER 2
## install nginx
```
sudo pacman -S nginx
```
##
```
sudo nvim /etc/nginx/nginx.conf
```
tambahkan ke paling bawah. sebelum tutup } terakhir
```
include /etc/nginx/sites-enabled/*;
```
<img width="309" height="391" alt="image" src="https://github.com/user-attachments/assets/e8dec74d-d76c-4ab6-b419-46fe4d7ea539" />

```
sudo mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
```
```
sudo nvim /etc/nginx/sites-available/omeka.conf
```
lalu isi
```
server {
    listen 80;
    server_name novaria.local.test;

    location / {
        proxy_pass http://ip statis untuk hederned:8081;

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
sudo nvim /etc/hosts
```
> tambahkan domain sesuai di nginx
```
server_ip novaria.local.test
```
```
sudo systemctl restart nginx
```
```
sudo systemctl restart nginx
```

akses ke browser
```
http://novaria.local.test
```

