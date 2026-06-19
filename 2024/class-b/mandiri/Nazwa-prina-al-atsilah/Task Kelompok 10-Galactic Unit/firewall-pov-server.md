# Firewall POV Server
## 1.   Mengecek apakah status ssh sudah Running atau belum
```
systemctl status sshd
```
Jika muncul text Active berwarna hijau, berarti sshd service sudah aktif.
## 2. Mengecek alamat ip yang aktif di servernya untuk diberikan kepada Admin
```
ip a
```
## 3.   Melihat seluruh konfigurasi Firewall Saat Ini
```
sudo firewall-cmd --list-all-zone
password for F123:
```
## 4. Keluar dari sesi
```
exit
```
