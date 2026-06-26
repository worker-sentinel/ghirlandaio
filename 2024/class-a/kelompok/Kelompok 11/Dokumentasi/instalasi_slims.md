# Instalasi Slims

## Rekam 

```bash
asciinema record namafile.cast
```

## Cek kernel aktif

```bash
uname -r
```

Karena yang digunakan adalah hardened maka yang muncul harus:

```bash
7.0.xx-hardened
```

---


## Sambungkan internet

```bash
iwctl
```

```bash
station wlan0 get-networks
```

```bash
station wlan0 connect nama wifi
```

Masukkan password Wi-Fi lalu:

```bash
exit
```

Cek koneksi:

```bash
ping 8.8.8.8
```

## . Pastikan podman sudah aktif

```bash
podman --version
```

Jika belum terinstall silahkan install dulu (Cek pada bagian install podman)

---


## . Download slims

```bash
wget https://github.com/slims/slims9_bulian/archive/refs/heads/master.zip
unzip master.zip
```

Kemudian Ekstrak file yang sudah diunduh

```bash
unzip master.zip
```

Untuk melihat daftar file

```bash
ls -lh
```

## Cek IP Address

```bash
ip addr
```

Untuk tes koneksi ke laptop database gunakan

```bash
ping -c 5 (ip addr laptop database)
```

Pastikan laptop data juga terhubung ke jaringan yang sama

---


```bash
cd slims9_bulian-master
```

Cek Isi Direktori

```bash
ls -lh
```

```bash
cp .env.example.env
```

```bash
cat .env
```

Pastikan Nilai HTTP_PORT dan DB_PORT sudah benar

Masuk ke pengeditan

```bash
nano docker-compose.yml
```

Edit pada bagian DB menjadi:

```bash
DB_HOST= (IP ADDRESS Laptop Database)
DB_PORT= 3306
DB_USER= nama
DB_PASS= password
DB_NAME= senayan
```

```bash
podman compose up -d
```

## Cek status

```bash
podman ps
```

Cek juga port yang ada, dibagian port sebelah status

Contoh

```bash
0.0.0.0:8881->80/tcp
```

Port itu yang akan dipakai untuk login menggunakan browser

http://IP Address laptop app:PORT

Contoh link yang digunakan untuk buka di browser

http://10.141.210.194:8881


## Upload Rekaman

ctrl + d untuk mengakhiri rekaman

```bash
asciinema upload namafile.cast
