# Mengecek apakah status ssh sudah Running atau belum
```
systemctl status sshd
```
Jika muncul text Active berwarna hijau, berarti sshd service sudah aktif.

# Mengecek alamat ip yang aktif di servernya untuk diberikan kepada Admin
```
ip a
```

# Melihat seluruh konfigurasi Firewall Saat Ini
```
sudo firewall-cmd --list-all-zone
```

# Keluar dari sesi
```
exit
```

