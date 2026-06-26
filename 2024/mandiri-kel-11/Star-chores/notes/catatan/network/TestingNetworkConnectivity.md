tools
```
ping
```
```
ip add
```
```
ip route
```

testing DNS

install package
```
sudo pacman -S bind
```
```
dig 8.8.8.8
dig google.com
```
```
nslookup google.com
```
```
host google.com
```
```
dig host server
```
```
dig @127.0.0.1 google.com
```

/etc/hosts


Berkas ini berfungsi sebagai resolusi DNS tahap pertama. Sebelum sistem melakukan kueri ke server DNS eksternal, sistem akan memeriksa daftar hostname dan alamat IP yang dipetakan secara manual di dalam berkas ini. Anda bisa menambahkan entri di sini untuk memblokir akses ke situs tertentu atau memetakan nama host lokal.

/etc/nsswitch.conf


Berkas ini menentukan urutan layanan yang digunakan sistem untuk melakukan pencarian host (nama komputer). Pada baris hosts:, Anda bisa melihat urutan prioritas, misalnya files (mengacu pada /etc/hosts) yang biasanya diletakkan paling depan, diikuti oleh metode pencarian lainnya seperti DNS.

/etc/resolv.conf


Berkas ini berisi daftar server nama (nameserver) yang digunakan sistem untuk menerjemahkan nama domain ke alamat IP. Penting untuk dicatat bahwa pada sistem modern yang menggunakan systemd-resolved, berkas ini sering kali dikelola secara otomatis dan tidak disarankan untuk disunting secara manual, karena perubahan akan ditimpa oleh layanan sistem.

