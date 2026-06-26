# Install Nginx
```
sudo pacman -s nginx
```
```
systemctl enable --now nginx
```
## Konfigurasi Nginx
```
sudo nvim /etc/nginx/nginx.conf
```
### Menghapus tanda '#'

> hapus tanda '#' pada #user https;

> lalu baris paling bawah sebelum '}' masukkan command
include /etc/nginx/sites-enabled/*;

## Membuat Direktori Virtual Host
```
sudo mkdir -p /etc/nginx/sites-available
```
```
sudo mkdir -p /etc/nginx/sites-enabled
```
## Membuat Konfigurasi Virtual Host
```
sudo nvim /etc/nginx/sites-available/slims.conf
```
insert

<img width="480" height="255" alt="65504a19-361f-424a-a9f6-a988a814c982" src="https://github.com/user-attachments/assets/a7b9b3ad-6f91-480b-b8cd-718eb9ab47fc" />


## Mengaktifkan Konfigurasi
```
sudo systemctl restart systemd-networkd
```
```
sudo ln -s /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
```
## Verifikasi dan Menjalankan Nginx
```
sudo nginx -t
```
```
ls -la /etc/nginx
```
```
sudo journalctl -xeu nginx.service
```
```
sudo nginx -t
```
```
sudo systemctl restart nginx
```
```
exit
```
