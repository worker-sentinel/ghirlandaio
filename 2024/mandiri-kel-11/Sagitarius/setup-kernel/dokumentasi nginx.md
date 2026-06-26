# Install nginx
## membuka file jaringan ethernet
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
## menginstall nginx
```
sudo pacman -S nginx
```
## membuat folder
```
sudo mkdir -p /etc/nginx/sides-available
```
```
sudo mkdir -p /etc/nginx/sides-enabled
```
## membuka file konfigurasi nginx
```
sudo nvim /etc/nginx/nginx.conf
```
## membuat file konfigurasi slims
```
sudo nvim /etc/nginx/sides-available/slims.conf
```
<img width="1280" height="963" alt="WhatsApp Image 2026-06-26 at 04 27 18" src="https://github.com/user-attachments/assets/02841226-7475-4eec-a3c1-1347b759b93c" />
## mengaktifkan konfigurasi slims di nginx
```
sudo ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
```
## mengecek konfigurasi nginx
```
sudo nginx -t
```
## restart nginx
```
sudo systemctl restart nginx
```
