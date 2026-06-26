# Setup Database

## Record

```bash
asciinema record namafile.cast
```

Cek IP Address 

```bash
ip addr
```

## Install Mariadb

```bash
pacman -Sy mariadb
```

```bash
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

## Aktifkan Mariadb
```bash
systemctl enable mariadb
```

## Menjalankan service mariadb
```bash
systemctl start mariadb
```

## Cek status mariadb
```bash
systemctl status mariadb
```

## Masuk ke mariadb
```bash
sudo maridb
```

## Membuat Database Slims

```bash
CREATE DATABASE senayan;
```

```bash
SHOW DATABASES;
```

## Membuat User untuk App Server

```bash
CREATE USER 'Kel11'@'10.141.210.194' IDENTIFIED BY 'kel11';
```

## Memberikan hak akses

```bash
GRANT ALL PRIVILEGES ON senayan.* TO 'Kel11'@'10.141.210.194';
```

## Menerapkan hak untuk akses masuk

```bash
FLUSH PRIVILEGES;
```

## Keluar dari mariadb

```bash
exit
```

## Impor database dari slims


```bash
mysql -u root -p senayan < /home/kel11/senayan.sql
```

Kemudian masuk lagi ke mariadb

```bash
sudo mariadb
```

```bash
USE senayan;
```

```bash
SHOW TABLES;
```

Jika tabelnya muncul, artinya tabel SLiMS berhasil diimpor ke database.


Kemudian kita pastikan juga port mariadb aktif

```bash
ss -tlnp | grep 3306
```

Dan kita buka akses mariadbnya (melalui jaringan)

```bash
firewall-cmd --permanent --zone=public --add-port=3306/tcp
```


```bash
firewall-cmd --reload

```

## Memverifikasi akses mariadb

```bash
firewall-cmd --zone=public --list-ports
```

Output yang keluar:

```bash
3306/tcp
```

## Menguji koneksi server app

```bash
ping -c 5 10.141.210.194
```

## Upload rekaman


exit dengan menekan ctrl + d

```bash
asciinema upload namafile.cast
```
