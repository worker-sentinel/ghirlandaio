

## 1. Konfigurasi Kernel Linux

Membuka file konfigurasi kernel untuk melakukan penyesuaian parameter sistem.

```bash
sudo nvim /etc/sysctl.d/99-zen.conf
```

---

## 2. Instalasi Nginx

Menginstal web server Nginx.

```bash
sudo pacman -S nginx
```

---

## 3. Konfigurasi Jaringan Ethernet

Membuka file konfigurasi jaringan ethernet.

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Simpan perubahan konfigurasi sesuai kebutuhan jaringan.

---

## 4. Konfigurasi IWD

Membuka file konfigurasi IWD (iNet Wireless Daemon).

```bash
sudo nvim /etc/iwd/main.conf
```

Menerapkan konfigurasi dengan merestart layanan IWD.

```bash
sudo systemctl restart iwd
```

---

## 5. Membuat Profil Hotspot

Masuk ke direktori penyimpanan konfigurasi IWD.

```bash
cd /var/lib/iwd/
```

Membuat atau mengedit profil hotspot.

```bash
sudo nvim myhotspot.ap
```

---

## 6. Menyesuaikan Konfigurasi Jaringan

Membuka kembali konfigurasi jaringan ethernet apabila diperlukan.

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

---

## 7. Menerapkan Konfigurasi Jaringan

Merestart layanan IWD.

```bash
sudo systemctl restart iwd
```

Merestart layanan systemd-networkd.

```bash
sudo systemctl restart systemd-networkd
```

Merestart layanan systemd-resolved.

```bash
sudo systemctl restart systemd-resolved
```

---

## 8. Menonaktifkan NetworkManager

Menonaktifkan layanan NetworkManager agar tidak konflik dengan IWD.

```bash
sudo systemctl disable NetworkManager
```

Menghentikan layanan NetworkManager.

```bash
sudo systemctl stop NetworkManager
```

---

## 9. Menjalankan Layanan IWD

Menjalankan layanan IWD.

```bash
sudo systemctl start iwd
```

Merestart layanan IWD untuk memastikan konfigurasi diterapkan.

```bash
sudo systemctl restart iwd
```

---

## 10. Membuat Access Point

Masuk ke utilitas IWD.

```bash
iwctl
```

Menampilkan perangkat jaringan yang terdeteksi.

```bash
device list
```

Mengubah mode perangkat Wi-Fi menjadi Access Point.

```bash
device wlan0 set-property Mode ap
```

Menjalankan profil hotspot yang telah dibuat.

```bash
ap wlan0 start-profile myhotspot
```

Mengedit konfigurasi hotspot apabila diperlukan.

```bash
nvim ap/myhotspot.ap
```

---

## 11. Keluar dari IWD

Keluar dari utilitas IWD.

```bash
exit
```
