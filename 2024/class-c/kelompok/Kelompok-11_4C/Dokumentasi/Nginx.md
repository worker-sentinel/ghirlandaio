## Memasang nginx di server2

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
**setelah tutup } di enter tambahkan**
```
include /etc/nginx/sites-enabled/*;
```
*kalau udh esc :wq*

```
nvim /etc/nginx/sites-avilable/atom.conf 
```
*diisi seperti di bawah ini*
```
server {
        listen 8080
        server_name atom.sebelas.test;
 
location / {
       proxy_pass http://(2.3.4.17):63001;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $schema;
       }
}
```
```
ln -sf /etc/nginx/sites-avilable/atom.conf /etc/nginx/sites-enabled/
```
```
systemctl restart nginx
```
## Selesai
