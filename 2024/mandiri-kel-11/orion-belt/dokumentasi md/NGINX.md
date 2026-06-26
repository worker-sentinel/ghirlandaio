NGINX berfungsi sebagai web server, untuk membuat server proxy

```
sudo pacman -S nginx
```

```
sudo cat /etc/systemd/network/20-wlan.network
```

```
sudo mkdir -P /etc/nginx/sites-available
```

```
sudo mkdir -P /etc/nginx/sites-enabled
```

```
sudo nvim /etc/nginx/nginx.conf
```
masuk ke mode insert (i)
pada bagian '#user http' (hastag dihapus)
lalu dibawah tanda #} ditambah:
include /etc/nginx/sites-enable/bintang*;
lalu esc dan :wq untuk write and quit

```
sudo nvim /etc/nginx/sites-available/slims.conf
```

masuk ke mode insert (i)
lalu isi bagian kosong dengan:

```
server   {
    listen 80;
    server_name slims.test;

    location / {
		proxy_pass http://[enp di server 1]:80;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $scheme;
	}
}

```
setelah itu esc, dan :wq untuk write and quit


symlink copy file slims.conf sites-available ke sites-enable

```
sudo ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
```

```
sudo nginx -t
```

```
sudo systemctl restart nginx
```

```
sudo nvim /etc/hosts
```

lalu cek di

```
http://slims.test
```

