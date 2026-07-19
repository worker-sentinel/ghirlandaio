# INSTALL MARIADB

## Instalasi paket

```
sudo pacman -S apache mariadb php php-fpm unzip php-apache php-intl php-mysqli imagemagick
```

## Masukin passwd

## Install paket tanpa php-mysqli

```
sudo pacman -S apache mariadb php php-fpm unzip php-apache php-intl imagemagick 
```
> Mengaktifkan Apache saat boot

```
sudo systemctl enable httpd
```
> Menjalankan Apache

```
sudo systemctl start httpd
```
> Menginisialisasi database MariaDB

```
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```
> Mengaktifkan MariaDB saat boot

```
sudo systemctl enable mariadb
```
> Menjalankan MariaDB

```
sudo systemctl start mariadb
```
> Login ke MariaDB

```
sudo mysql -u root -p
```
> Masukin password

# Membuat database

```
create database comet
``` 
> Membuat user database

```
create user 'comet'@'localhost' identified by 'catchers';
```
> Memberikan hak akses

```
grant all privileges on comet.* to 'comet'@'localhost';
```
> Menyimpan perubahan hak akses

```
flush privileges;
```
> Keluar dari MariaDB

```
quit
```
