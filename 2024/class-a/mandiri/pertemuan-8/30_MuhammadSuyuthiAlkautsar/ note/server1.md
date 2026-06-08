
download packet apache
```
sudo pacman -Syu
sudo pacman -S apache
```

https://archlinux.org/packages/?name=apache


Aktifkan service
```
sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd
```

test dengan mengetik 
```
curl http://localhost/
```
seharusnua muncul page default index.html
```
cd /srv/http
sudo touch index.html
sudo nano index.html
```
isi
```html
<h1>Hallo mas fuad</h1>
```
```
curl http://localhost/
```
https://www.youtube.com/watch?v=YaEdcKWiJEk

Konfigurasi apache untuk file terletak di
```
/etc/httpd/conf
```

Konfigurasi apache untuk file utama terletak di
```
/etc/httpd/conf/httpd.conf
```
https://wiki.archlinux.org/title/Apache_HTTP_Server
Pastikan MPM Event dan modul berikut aktif:


<img width="492" height="126" alt="image" src="https://github.com/user-attachments/assets/60bd6bcf-88d1-497b-b44c-4dd774ebcdc6" />


```
LoadModule mpm_event_module modules/mod_mpm_event.so
```
```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
LoadModule rewrite_module modules/mod_rewrite.so
```
```
ServerName localhost:80 //boleh pake ip juga seperti 192.168.100.52
Include conf/extra/httpd-vhosts.conf
```
secara default dia akan mengkofigurasi folder di
```
/srv/http
```
Create /etc/httpd/conf/extra/php-fpm.conf with the following content:
```
DirectoryIndex index.php index.html
<FilesMatch \.php$>
    SetHandler "proxy:unix:/run/php-fpm/php-fpm.sock|fcgi://localhost/"
</FilesMatch>
```
And include it at the bottom of /etc/httpd/conf/httpd.conf:
```
Include conf/extra/php-fpm.conf
```
These options in /etc/httpd/conf/httpd.conf might be interesting for you:
```
Listen 80
```
masukan ```127.0.0.1:80``` jika ingin local development yang hanya bisa diakses lewat computer



konfigurasi virtual host, Tambahkan
```
<VirtualHost *:80>
    ServerName arteri.local
    DocumentRoot "/srv/http/arteri"

    <Directory "/srv/http/arteri">
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
        DirectoryIndex index.php index.html
    </Directory>

    <DirectoryMatch "^/srv/http/arteri/(application|system|sql)">
        Require all denied
    </DirectoryMatch>

    <FilesMatch "\.php$">
        SetHandler "proxy:unix:/run/php-fpm-legacy/php-fpm.sock|fcgi://localhost/"
    </FilesMatch>

    ErrorLog "/var/log/httpd/arteri-error.log"
    CustomLog "/var/log/httpd/arteri-access.log" combined
</VirtualHost>
```
```
sudo apachectl configtest
sudo systemctl httpd
sudo systemctl reload httpd
```
https://httpd.apache.org/docs/current/zh-cn/programs/apachectl.html


https://httpd.apache.org/docs/current/zh-cn/sitemap.html

```
"/srv/http"
```
masukan folder web di sini


https://wiki.archlinux.org/title/Apache_HTTP_Server#Using_php-fpm_and_mod_proxy_fcgi

php-fpm-legacy

```
sudo pacman -S php-fpm-legacy
```
aktifkan
```
sudo systemctl enable php-fpm-legacy
sudo systemctl start php-fpm-legacy
sudo systemctl status php-fpm-legacy
```
cek
```
sudo systemctl is-active php-fpm-legacy
```
```
cd /srv/http
```
```
sudo nano test.php
```
isi
```
<?
  phpinfo();
?>
```
cek webnya akan muncul info php-fpm sudah nyala atau belum
curl http://localhost/test.php
```
buka
```
```
sudo nano /etc/php-legacy/php.ini
```
Cari dengan Ctrl + W.


aktifkan dengan menghapus ;
```
extension=mysqli
extension=pdo_mysql
extension=gd
extension=zip
extension=mbstring
```
Restart php:
```
sudo systemctl restart php-fpm-legacy
```

```
sudo systemctl enable --now php-fpm
```
https://www.youtube.com/watch?v=3YJpmEMk8P0

Siapkan MariaDB
```
sudo pacman -S mariadb
```
```
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```
```
sudo systemctl enable mariadb
sudo mariadb-secure-installation
```
`/etc/my.cnf.d/server.cnf`:

```
[mysqld]
bind-address = 127.0.0.1
```

3. Buat Database dan User

```
sudo mariadb
```

```
create database arteri_db character set utf8 collate utf8_unicode_ci;
create user 'arteri_user'@'localhost'
  identified by 'kautsar123';
grant all privilleges on arteri_db.* to 'arteri_user'@'localhost';
flush privileges;
exit;
```

```
sudo systemctl restart mariadb
sudo ss -ltnp | grep 3306
```

MariaDB seharusnya mendengarkan pada `127.0.0.1:3306`, bukan `0.0.0.0:3306`.


https://github.com/dicarve/arteri/blob/master/panduan_instalasi.md

Ambil Project arteri dari GitHub
```
cd /srv/http
```
Clone project arteri:
```
sudo git clone https://github.com/dicarve/arteri.git arteri
```
Masuk folder arteri:
```
cd arteri
```
Set ownership ke user Apache:
```
sudo chown -R http:http /srv/http/arteri
```
Set permission dasar:
```
sudo chown -R root:http /srv/http/arteri
sudo find /srv/http/arteri -type d -exec chmod 750 {} +
sudo find /srv/http/arteri -type f -exec chmod 640 {} +

sudo chown -R http:http \
  /srv/http/arteri/application/cache \
  /srv/http/arteri/application/logs \
  /srv/http/arteri/files
sudo chmod 750 \
  /srv/http/arteri/application/cache \
  /srv/http/arteri/application/logs \
  /srv/http/arteri/files
```
Import struktur database bawaan Arteri:
```
sudo mariadb -u arteriuser -p arteri < /srv/http/arteri/sql/arteri.sql
```
Masukkan password:
```
kautsar123
```
```
mariadb -u arteri_user -p -D arteri_db -e show tables;
```
Edit database config Arteri
```
sudo nano /srv/http/arteri/application/config/database.php
```
Isi bagian pentingnya harus seperti ini:
```
$active_group = 'default';
$query_builder = TRUE;

$db['default'] = array(
    'dsn'      => '',
    'hostname' => 'localhost',
    'username' => 'arteri_user',
    'password' => 'GANTI_DENGAN_PASSWORD_KUAT',
    'database' => 'arteri',
    'dbdriver' => 'mysqli',
    'dbprefix' => '',
    'pconnect' => FALSE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_unicode_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```
set permission
```
sudo chown root:http /srv/http/arteri/application/config/database.php
sudo chmod 640 /srv/http/arteri/application/config/database.php
```
https://wiki.archlinux.org/title/MariaDB

Konfigurasi firewalld

```bash
sudo systemctl enable --now firewalld
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

Jika HTTPS sudah dikonfigurasi:

```bash
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

Jangan buka MariaDB ke jaringan:

```bash
sudo firewall-cmd --permanent --remove-service=mysql
sudo firewall-cmd --permanent --remove-port=3306/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```
```
sudo systemctl restart mariadb
sudo systemctl restart php-fpm-legacy
sudo systemctl restart httpd
```

Patch Kompatibilitas CodeIgniter 3.1.6

### Dynamic property PHP 8.3

Pada `/srv/http/arteri/index.php`, ubah:

```
case 'development':
    error_reporting(-1);
    ini_set('display_errors', 1);
break;
```

menjadi:

```
case 'development':
    error_reporting(E_ALL & ~E_DEPRECATED & ~E_USER_DEPRECATED);
    ini_set('display_errors', 1);
break;
```

CodeIgniter 3.1.6 dibuat sebelum PHP 8.2. Tanpa patch ini, notifikasi `Creation of dynamic property ... is deprecated` tercetak sebelum session dan redirect, lalu memicu `headers already sent`.

https://github.com/bcit-ci/CodeIgniter/issues/6278

https://www.codeigniter.com/userguide3/libraries/sessions.html

### Path session

Pada `/srv/http/arteri/application/config/config.php`, ubah:

```
$config['sess_save_path'] = NULL;
```

menjadi:

```
$config['sess_save_path'] = APPPATH.'cache';
```
```APPPATH``` 3 P
```
sudo chown http:http /srv/http/arteri/application/cache
sudo chmod 750 /srv/http/arteri/application/cache
```
https://firewalld.org/



referensi


https://www.youtube.com/watch?v=ZpazIwFMqY8


https://www.youtube.com/watch?v=zsDzrwT7nvk&t=348s


https://www.youtube.com/watch?v=VKtOhvwlNgs&t=37s


https://www.youtube.com/watch?v=GYnmm97bPxg


https://httpd.apache.org/docs/current/zh-cn/sitemap.html

https://www.redhat.com/en/blog/linux-file-permissions-explained


