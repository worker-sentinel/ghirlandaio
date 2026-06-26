## Install Nginx

```
sudo pacman -S nginx
```
```
sudo systemctl enable --now nginx
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
mkdir -p /etc/nginx/sites.available
```
```
mkdir -p /etc/nginx/sites.enabled
```
```
nvim /etc/nginx.conf
```
lalu masukin syntax diakhir 
```
include /etc/nginx/sites-enabled/;*
```
esc :wq
```
nvim /etc/nginx/sites-available/slims.conf
```
lalu masukan    
```
server {
    listen 8080;
    sever_name slims.stardust.test;

    location / {
        proxy_pass http://8.8.8.3:63001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $schema;
    }
}
```
```
ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
