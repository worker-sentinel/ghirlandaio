### Koneksi ke server

```
ssh F123@192.168.100.78
```

Digunakan untuk login ke server menggunakan akun `F123` pada IP `192.168.100.78`.

### Cek kernel yang digunakan

```
uname -a
```

Digunakan untuk melihat informasi sistem dan memastikan kernel yang sedang aktif.

### Install Podman Compose

```
sudo pacman -S podman-compose
```

Digunakan untuk menginstall Podman Compose yang nantinya dipakai menjalankan container dari file compose.

### Download repository

```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

Digunakan untuk mengunduh file repository dari internet.

### Ekstrak file ZIP

```
unzip master.zip
```

Digunakan untuk mengekstrak isi file `master.zip`.

### Lihat isi direktori

```
ls
```

Digunakan untuk melihat file dan folder yang ada pada direktori saat ini.

### Masuk ke folder project

```
cd docker-compose-for-slims-master
```

Digunakan untuk berpindah ke folder project SLiMS.

### Lihat isi folder project

```
ls
```

Digunakan untuk memastikan file project berhasil diekstrak.

### Ubah permission folder SLiMS

```
sudo chmod -R 777 app/slims
```

Digunakan untuk memberikan hak akses penuh ke folder SLiMS agar dapat dibaca dan ditulis oleh sistem.

### Menjalankan container

```
sudo podman compose up -d
```

Digunakan untuk menjalankan seluruh service yang telah didefinisikan pada file compose.

---

### Edit konfigurasi registries Podman

```
sudo nvim /etc/containers/registries.conf
```

Digunakan untuk membuka file konfigurasi registries Podman menggunakan Neovim.

Setelah file terbuka, hapus tanda `#` pada bagian `unqualified-search-registries`, lalu simpan perubahan dengan `:wq`.


### Melihat container

```
sudo podman ps -a
```

Digunakan untuk melihat seluruh container beserta statusnya.

### Melihat IP address

```
ip a
```

Digunakan untuk melihat konfigurasi jaringan dan alamat IP yang dimiliki server.

### Akses SLiMS melalui browser

```
http://IP_SERVER:80
```

Digunakan untuk membuka halaman SLiMS melalui browser menggunakan IP server yang diperoleh dari perintah `ip a`.

# Konfigurasi IP Statis Server 1

### Membuka file konfigurasi jaringan

```
sudo nvim /etc/systemd/network/20-ethernet.network
```

Digunakan untuk membuat atau mengubah konfigurasi jaringan agar menggunakan IP statis.


### Mengisi konfigurasi jaringan

```
[Match]
Type=ether

[Network]
Address=23.23.2.3/24
Gateway=23.23.2.1
DNS=1.1.1.1 8.8.8.8
```

Digunakan untuk menentukan alamat IP, gateway, dan DNS yang akan digunakan oleh server.


### Menerapkan konfigurasi jaringan

```
sudo systemctl restart systemd-networkd
```

Digunakan untuk memulai ulang layanan jaringan agar konfigurasi baru diterapkan.

### Verifikasi IP statis

```
ip a
```

Digunakan untuk memastikan alamat IP statis berhasil terpasang pada server.

### Bukti Slims bisa diakses
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/ba3474db-cfcb-4b54-be03-626514a6b2aa" />

