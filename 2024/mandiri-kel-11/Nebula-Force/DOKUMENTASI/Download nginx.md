## Memasang nginx di server2
```
sudo pacman -S nginx
```
## Mengaktifkan Nginx
```
sudo systemctl enable –now nginx
```
## Mengecek Status Nginx
```
sudo systemctl status nginx
```
**untuk melihat apakah layanan Nginx sudah berhasil berjalan atau belum. Jika statusnya active (running) berarti Nginx sudah aktif.**

## Menjalankan Nginx
```
sudo systemctl start nginx
```
```
sudo su 
```
## Membuat folder available-enable
```
mkdir -p /etc/nginx/sites-avilable
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
*kalau udah esc lalu :wq*

```
nvim /etc/nginx/sites-avilable/slims.conf
```
**diisi seperti di bawah ini**
```
server {
        listen 8080
        server_name slims.sebelas.test;
 
location / {
       proxy_pass http://(2.3.4.17):63001;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $schema;
       }
}
```
## Mengaktifkan Konfigurasi
```
ln -sf /etc/nginx/sites-avilable/slims.conf /etc/nginx/sites-enabled/
```
```
systemctl restart nginx
```
## Selesai
