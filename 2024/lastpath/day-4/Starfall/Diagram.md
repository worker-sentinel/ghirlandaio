## Diagram Regional

<img width="798" height="450" alt="WhatsApp Image 2026-07-03 at 12 06 40 PM" src="https://github.com/user-attachments/assets/5d83d0a7-bef8-4fca-be32-a2479a49be1a" />


Dalam proses instalasi SLiMS, terdapat 5 region yang perlu disetup yang terdiri dari Control, Public, Internal, Data, dan Client.
Masing-masing region memiliki alur kerjanya sendiri:

### 1. Control 
Region ini untuk manajemen dan monitoring. Tempat dimana menjadi sebagai admin yang melakukan SSH ke semua server, mengatur firewall dan memantau log. 
Region ini paling dibatasi aksesnya dan hanya bisa diakses dari IP/host tertentu (hanya di pemilik laptop itu sendiri (local)). 
Karena kalau sampai bocor IP/host nya, seluruh sistem bisa diretas oleh cracker.

### 2. Public
Region ini ditempatkan reverse proxy yang meneruskan request acces dari client ke server aplikasi di region Internal. 

### 3. Internal
Di sini adalah tempat SLiMS berjalan. Disebut internal karena server ini tidak langsung tereskpos ke internet, harus lewat region Public terlebih dahulu.

### 4. Data 
Zona khusus untuk database server kaya mariadb/mysql untuk menyimpan data informasi pemustaka misalnya. Region ini diisolasi dari akses publik (khalayak umum)
karena berisi informasi data yang penting dan harus dilindungi. 

### 5. Client
Region Client itu ibarat adalah user. Komputer/laptop yang bisa diakses oleh user untuk mengakses SLiMS lewat browser. Tung tung tung sahur
