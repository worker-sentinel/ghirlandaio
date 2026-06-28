# nginx server data
## mengaktifkan user namespace

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
mkdir -P /etc/nginx/sites-evailable
```
```
mkdir -P /etc/nginx/sites-enabled
```
```
nano /etc/nginx/nginx.conf
```

ketik: include /etc/nginx/sites.available/slims.conf

ketik: include /etc/nginx/sites-enabled/*;

```
mkdir -P /etc/nginx/sites-available
```
```
sudo mkdir -P /etc/nginx/sites-available
```
```
sudo mkdir -P /etc/nginx/sites-enabled
```
```
sudo nano /etc/nginx.conf
```

ketik: 
server {
        listen 8080
        server_name slims.comets.test;

location / {
        proxy_pass http://(2.3.4.17):63081;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $pro
        proxy_set_header X-Forwaded-Proto $schema;
        }
}

```
ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enable/
```
```
systemctl restart nginx
```
```
sudo nginx -t
```
```
cat /etc/nginx/sites-available/slims.conf
```
```
tail -20 /etc/nginx/nginx.conf
```
```
grep -n "include /etc/nginx/sites-enabled" /etc/nginx/nginx.conf
```
```
grep -n "^}" /etc/nginx/nginx.conf
```
```
sudo snd -i '119d' /etc/nginx/nginx.conf
```
```
sudo nginx -t
```
```
sudo nano /etc/nginx/nginx.conf
```
```
sudo su
```
```
sudo pacman -Rns nginx
```
```
sudo nvim /etc/nginx/nginx.conf
```

ini nginx nya masih gagal
