# Instalasi ATOM

## Instalasi & Hardening MySQL
> Atom yang akan diinstall adalah atom versi 2.10 sehingga ia membutuhkan MySQL versi 8.0 ke atas karena atom versi tersebut menggunakan fitur bernama CTE sehingga apabila MySQL nya masih versi di bawah 8.0 maka akan menyebabkan eror saat proses instalasi atom. 

> Update daftar package-nya sebelum mengunduh MySQL dengan cara:
```
sudo apt update
sudo apt install -y mysql-server
```

> MySQL yang sudah terinstall default-nya masih belum aman, misal masih ada akun anonymous, remote root login masih menyala, dan lainnya sehigga perlu menjalankan:
```
sudo mysql_secure_installation
```

> Untuk setting tambahan
```
/etc/mysql/conf.d/mysql.cnf
```
> Setelah itu, tambahkan:
```
[mysqld]
sql_mode=ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
optimizer_switch=''block_nested_loop=off'
```
> Hal ini untuk mengatur bagaimana MySQL meng-handle data & query agar menyesuaikan dengan kebutuhan Atom

> Agar hasil settingan digunakan, lakukan restart
`````
sudo systemctl restart mysql
`````

## Install Elasticsearch
> Elasticsearch merupakan mesin pencari internal milik Atom yang digunakan untuk mencari dokumen atau arsip di Atom. Karena Elasticsearch berjalan di atas Java, maka perlu melakukan instalasi Java terlebih dulu:
```
sudo apt install -y openjdk-110-jre-headless apt-transport-https software-properties-common
wget -q0 - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```

> Tambahkan repository resmi
```
echo "deb[signed-by=/usr/share//keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/oss-7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```
> Ini untuk menambahkan repository Elasticsearch ke daftar sumber install Ubuntu sehingga ketika ```apt install```, Ubuntu tahu harus mengambil dari mana.

> Mulai instalasi Elasticsearch
```
sudo apt update
sudo apt install -y elasticsearch-oss
```

> AKtifkan ELasticsearch dan buat menjadi auto-start ketika booting
```
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```
> ```enable``` dilakukan biar setiap kali laptop menyala, Elasticsearch akan otomatis berjalan sendiri, sementara ```start``` untuk menyalakan Elasticsearch untuk saat itu juga.

## PHP
> Atom merupakan aplikasi web yang dibangun dengan menggunakan PHP. Ubuntu 24.04 sudah memiliki PHP 8.3 bawaan sehingga hanya perlu install beserta ekstensi-ekstensi yang memang wajib digunakan oleh Atom.
```
sudo apt install -y php-common php8.3-common php8.3-cli php8.3-curl php-json php8.3-ldap php8.3-mysql php8.3-readline php8.3-xml php8.3-mbstring php8.3-xsl php8.3-zip php-apcu
```
> Itu semua adalah modul tambahan agar PHP bisa berinteraksi dengan MySQL, membaca XML, konseksi LDAP, dan sebagainya sesuai dengan kebutuhan Atom.

## German Job Server
> German merupakan pengatur antrian tugas yang artinya jika ada proses berat di Atom seperti import data dengan ukuran besar, maka akan dipindahkan ke German agar bisa dikerjakan di background sehingga tidak akan membuat website nge-lag.
```
sudo apt install -y gearman-job-server
```

## Paket Tambahan
> Atom butuh untuk membuat PDF untuk membuat panduan/daftar arsip maka diperlukan Apache FOP karena dia yang akan mengerjakan
```
sudo apt install -y --no-install-recommends fop libsaxon-java
```
> ```--no-install-recommends``` diketik agar apt hanya mengunduh yang benar-benar dibutuhkan dan tidak perlu mengunduh paket rekomendasi tambahan. Hal ini penting dilakukan karena apabila tidak mengetik command tersebut, maka ```openjdk-8-jre``` akan ikut keunduh. Kita tidak perlu karena sebelumnya sudah diunduh saat melakukan instalasi Elasticseacrh sehingga mencegah penumpukan Java yang tidak terpakai.

> Pastikan default Java sudah versi 11 dengan cara:
```
sudo update-java-alternatives -s java-1.11.0-openjdk-amd64
```
> Untuk memilih Java mana yang akan digunakan apabila di sistem ada beberapa versi Java. Apabila pada tahap ini muncul error, maka abaikan saja.

> Untuk mengecek Java apa saja yang sudah terinstall
```
sudo update-java-alternatives -l
sudo update-alternatives --get-selections | grep java
```

> Apabila menginginkan Atom untuk bisa memproses objek digital misal dari JPEG maka perlu mengunduh paket opsional
```
sudo apt install -y imagemagick ghostscript poppler-utils ffmpeg
```

## Download Atom
> Download Tarball
```
wget https://storage.accesstomemory.org/releases/atom-latest.tar.gz
sudo mkdir -p /usr/share/nginx/atom
sudo tar xzf atom-latest.tar.gz -C /usr/share/nginx/atom --strip 1
```
>```wget``` untuk mendownload file Atom versi terbaru, ```mkdir -p``` untuk membuat folder untuk menyimpan Atom, ```tar xzf atom-latest.tar.gz -C /usr/share/nginx/atom --strip 1``` untuk extract file tarball-nya ke folder yang sudah dibuat tadi. ```--strip 1``` itu untuk menghilangkan folder pembungkus yang ada di dalam tar-nya agar file langsung masuk di ```/usr/share/nginx/atom```

## Membuat Database
```
sudo mysql -h loacalhost -u root -p -e "CREATE DATABASE atom CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;"
```
> ```-h localhost``` merupakan server MySQL di local.
> ```-u root``` agar login menjadi user root
> ```-p``` untuk membuat password
> nama database di sini dinamakan ```atom``` sehingga bebas boleh diganti apapun

> Pastikan database yang digunakan itu kosong sehingga perlu dilakukan drop
```
DROP DATABASE atom;
```

## Membuat User untuk Atom
```
sudo mysql -h localhost -u root -p -e "CREATE USER 'atom'@'localhost' IDENTIFIED BY '12345';"
sudo mysql -h loacalhost -u root -p -e "GRANT ALL PRIVILEGES ON atom.* TO 'atom'@'localhost';"
```
> Artinya username nya adalah ```atom``` dengan password ```12345``` dan diberikan akses penuh ```(ALL PRIVILEGES)``` namun hanya sebatas ke database ```atom``` saja.
> Hak akses untuk ```INDEX, CREATE, ALTER``` hanya dibutuhkan ketika proses install atau upgrade saja. Setelah Atom berjalan dengan normal, privilege tersebut bisa dicabut lagi atau menggantinya ke user yang lebih terbatas di ```config.php``` demi keamanan.

## Menjalankan Installer Atom
```
cd /usr/share/nginx/atom
sudo php symfony tools:install
```
> Ini akan menyetting Atom sesuai dengan environment, membuat tabel-tabel yang dibutuhkan, serta membuat index untuk Elasticsearch. Saat dijalankan, maka akan diberikan beberapa hal seperti berikut:
```
Database host: localhost
Database port: 3306
Database name: atom
Database user: atom
Database password: 12345
Search host: localhost
Search port: 9200
Search index: atom
```

> Sisanya opsional yang artinya bebas sesuai kebutuhan seperti:
```
Site title
Site description
Site base URL
Admin email
Admin username
Admin password
```
