## 1. Cek hostname

```
hostnamectl
```
digunakan untuk menampilkan informasi lengkap mengenai identitas sistem (hostname) beserta informasi dasar sistem operasi.  

Fungsinya untuk Mengetahui nama host yang sedang digunakan dan Memastikan server masih menggunakan hostname bawaan atau sudah sesuai dengan pembagian region.

---

## 2. Mengganti hostname menjadi control `

```
sudo hostnamectl set-hostname control
```
Perintah

 `hostnamectl set-hostname` 

digunakan untuk mengubah nama host secara permanen. 

Fungsinya untuk  Memberikan identitas server sesuai dengan pembagian region.
---

## 3. Memastikan Hostname Sudah Berubah

```
hostname
```
hanya menampilkan nama host yang sedang aktif.

Sedangkan

```
hostnamectl
```
menampilkan informasi sistem secara lebih lengkap beserta hostname yang baru.

Fungsinya untuk  Menghindari kesalahan identitas server sebelum melakukan konfigurasi berikutnya.

---


## 4. Update package

```
sudo pacman -Syu
```
Perintah `pacman` merupakan package manager bawaan Arch Linux yang digunakan untuk mengelola paket perangkat lunak.

Fungsinya untuk Memperbarui sistem ke versi terbaru dan Memperbaiki bug.



## 5. Aktifkan SSH

```
sudo systemctl enable sshd
```

digunakan agar layanan SSH otomatis berjalan setiap kali sistem dinyalakan.

```
sudo systemctl start sshd
```

digunakan untuk langsung menjalankan layanan SSH tanpa perlu me-restart komputer.

---

## 6. Cek status SSH

```
systemctl status sshd
```


atau

```
systemctl is-active sshd
```

Kalau muncul

```
active
```

berarti layanan SSH telah aktif dan siap menerima koneksi dari perangkat lain.

---

## 7. Cek user

```
whoami
```
Perintah `whoami` digunakan untuk menampilkan nama pengguna (user) yang sedang aktif pada terminal.

---

## 8. Cek kernel

```
uname -r
```
Perintah `uname -r` digunakan untuk menampilkan versi kernel Linux yang sedang digunakan.

---

## 10. Cek distro

```
cat /etc/os-release
```
Perintah ini digunakan untuk menampilkan isi file `/etc/os-release`, yaitu file yang berisi informasi identitas sistem operasi Linux.
