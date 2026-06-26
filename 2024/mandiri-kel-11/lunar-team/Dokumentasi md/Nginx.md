# Dokumentasi Pemasangan Nginx
## Pemasangan Nginx di Server 2
```
sudo pacman -S nginx
```
```
sudo systemctl enable –now nginx
```
```
sudo systemctl status nginx
```
```
sudo systemctl start nginx
```
```
sudo su 
```
```
mkdir -p /etc/nginx/sites-available
```
```
mkdir -p /etc/nginx/sites-enabled
```
```
nvm /etc/nginx/nginx.conf
```
```
include /etc/nginx/sites-enabled/*;
```
```
Esc :wq
```
```
nvim /etc/nginx/sites-avilable/atom.conf 
```
```
server {
        listen 8080;
        server_name atom.local;
 
location / {
       proxy_pass http://10.10.10.2:63001;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
       }
}
```
```
ln -sf /etc/nginx/sites-avilable/atom.conf /etc/nginx/sites-enabled/
```
```
systemctl restart nginx
```
