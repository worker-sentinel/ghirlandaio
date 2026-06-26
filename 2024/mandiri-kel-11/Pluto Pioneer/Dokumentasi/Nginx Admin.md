## Instal nginx
```
sudo pacman -S nginx
```
## 
```
sudo mkdir -p /etc/nginx/sites-available
sudo mkdir -p /etc/nginx/sites-enable
```
## Konfigurasi nginx
```
sudo nvim /etc/nginx/nginx.conf
```
hapus pagar pada user http, lalu scroll ke paling bawah ketik
```
include /etc/nginx/sites-enabled/*;}
```

## Konfigurasi nginx slims
```
sudo nvim /etc/nginx/sites-enable/slims.conf
```

## Membuat Konfigurasi Slims
``` 
sudo ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enable
```

## Menguji Konfigurasi Nginx
```
sudo nginx -t 
```

## Restart nginx
```
sudo systemctl restart nginx
```
