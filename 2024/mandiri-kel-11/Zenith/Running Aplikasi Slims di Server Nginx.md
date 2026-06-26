# **Menjalankan Aplikasi SLiMS di Server Nginx**

## **1. Pastikan Koneksi LAN**
Pastikan kabel LAN telah terhubung antara server.

---

## **2. Restart Layanan Jaringan**
Jalankan perintah berikut pada server Nginx.

```bash
sudo systemctl restart systemd-networkd
```

---

## **3. Periksa Konfigurasi Jaringan**
Buka file konfigurasi jaringan.

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Periksa alamat IP server.

```bash
ip a
```

---

## **4. Periksa Konfigurasi Nginx**
Pastikan file konfigurasi SLiMS tersedia dan memiliki isi.

```bash
sudo nvim /etc/nginx/sites-available/slims.conf
```

Uji konfigurasi Nginx.

```bash
sudo nginx -t
```

Jika tidak terdapat error, restart layanan Nginx.

```bash
sudo systemctl restart nginx
```

---

## **5. Periksa Konfigurasi Hosts**
Pastikan file **hosts** telah dikonfigurasi.

```bash
sudo nvim /etc/hosts
```

---

## **6. Pengujian**
Buka browser pada server Nginx, kemudian akses alamat berikut.

```text
http://[nama-localhost-yang-sudah-dibuat]
```

Contoh:

```text
http://slims.local
```

<img width="1600" height="812" alt="image" src="https://github.com/user-attachments/assets/aa92d8d1-64f8-4047-bfb5-d1f2edf67b9a" />


