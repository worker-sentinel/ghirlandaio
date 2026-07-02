# Install Apache

```
sudo pacman -S apache
```

setelah itu install php-fpm

```
sudo pacman -S php php-fpm
```

```
sudo systemctl enable --now php-fpm
```

setelah itu cek status 
```
sudo systemctl status php-fpm
```

lalu keluar

```
exit
```
