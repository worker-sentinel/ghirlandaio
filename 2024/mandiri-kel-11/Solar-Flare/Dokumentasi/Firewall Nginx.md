# Firewall Nginx

## 1. Nambahin rule
> port 8080

```
firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.15/24" port port="8080" protocol="tcp" accept'success
```

> port 22

```
firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.16/24" port port="22" protocol="tcp" accept'success
```

## 2.Struktur direktori
> bikin folder untuk file dalam direktori

```
mkdir -p /etc/nginx/sites-available
```

```
mkdir -p /etc/nginx/sites-enabled
```

## 3.Buka file config
```
nvim /etc/nginx/nginx.conf
```
> sebelum edit klik i kecil
> edit, apus tanda pagar di user http:
> nah, dibawah event {} tambahin http { ntar tampilannya gini nih:
```
http {
    types_hash_max_size 4096;
    types_hash_bucket_size 128;
```
> terus panah bawah, sebelum tanda { terakhir, tambahin include /etc/nginx/sites-enabled/*;
> selese edit, klik Esc terus ketik :wq

# 4.Buka file config slims
```
nvim /etc/nginx/sites-available/slims.conf
```
> buat edit, klik i
> terus ketik:

```
server {
   listen 80;
   server_name contoh.local;

   location / {
       proxy_pass http://10.10.1.20:8080;

       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwaded-For $proxy_add_x_forwaded_for;
       proxy_set_header X-Forwaded-Proto $schme;
   }
}
```

> klik Esc, terus ketik :wq

## 5.Aktifin ulang config slimsny
```
ln -s /etc/nginx/sites-available/slims.conf /etc/nginx/etc/sites-enabled/
```

> Masuk ke direktori konfig nginx
```
cd /etc/nginx
```

> Masuk ke folder sites-enabled
```
cd sites-enabled
```

> Cek folder
```
ls -la
```

> Hasil jika link berhasil dibuat muncul:
```
slims.conf -> /etc/nginx/sites-available/slims.conf
```

> Cek konfig nginx
```
nginx -t
```

> Cek status nginx
```
systemctl status nginx
```

## 6.Matiin Apache
```
syystemctl disable httpd
```

## 7.Restart nginx
```
sysytemctl restart nginx
```

## 8.Cek ulang status ngingx
> kalau muncul:
```
Active: active (running)
```
> itu udeh berhasil

## 9.Aktifin Ngingx pas boot
```
systemctl enable nginx
```
> ntar muncul: Created symlink

## 10.Cek lgi
```
systemctl status ngingx
```
> klo kliatan Loaded: loaded (...) enabled, udah itu udh jalan nginx nya
> abis tu ketik exit aja yey







