# Setup IP Admin & IP Operator

```
useradd -m operator
```

```
passwd operator
```
Set password untuk user operator.

```
iwctl
```

```
pacman -S networkmanager
```
Install NetworkManager untuk manajemen koneksi jaringan.

```
systemctl enable NetworkManager
```

```
systemctl start NetworkManager
```

```
systemctl status NetworkManager
```
Cek status NetworkManager, pastikan active (running).

```
nmtui
```
Buka Network Manager TUI untuk konek ke jaringan secara interaktif.

```
systemctl restart NetworkManager
```
Restart NetworkManager agar koneksi diterapkan.

```
nmcli device status
```
Cek status semua network device yang terdeteksi.

```
ip a
```
Cek IP address yang aktif di semua interface.

```
nmcli connection add type ethernet ifname enp0s31f6 con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.10.10/24 ipv4.gateway 10.10.10.1 ipv4.dns 8.8.8.8
```

```
echo "nmcli connection up admin-connection" >> /home/gass/.bash_profile
```
```
cat /home/gass/.bash_profile
```
Verifikasi isi .bash_profile user gass.

```
nmcli connection add type ethernet ifname enp0s31f6 con-name "operator-connection" ipv4.method manual ipv4.addresses 10.10.10.10/24 ipv4.gateway 10.10.10.1 ipv4.dns 8.8.8.8
```

```
echo "nmcli connection up operator-connection" >> /home/gass/.bash_profile
```
```
cat /home/operator/.bash_profile
```
```
usermod -aG wheel operator
```
Tambahkan user operator ke grup wheel agar bisa menggunakan sudo.

```
exit
```
Keluar dari sesi root.

---

```
ip a
```

```
nmcli connection add type ethernet ifname enp0s31f6 con-name "operator-connection" ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4.gateway 10.10.10.1 ipv4.dns 8.8.8.8
```
Buat ulang koneksi operator dengan IP yang benar yaitu 10.10.10.11.

```
nmcli connection delete "operator-connection"
```
Hapus koneksi operator yang lama karena konfigurasinya salah.

```
nmcli connection add type ethernet ifname enp0s31f6 con-name "operator-connection" ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4.gateway 10.10.10.1 ipv4.dns 8.8.8.8
```
Buat ulang koneksi operator dengan konfigurasi yang sudah benar.

```
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```
Tambahkan perintah aktivasi koneksi ke .bash_profile user operator agar otomatis aktif saat login.

```
cat /home/operator/.bash_profile
```
Cek isi .bash_profile operator, pastikan tidak ada duplikat.

```
nvim /home/operator/.bash_profile
```

```
cat /home/operator/.bash_profile
```

## Referensi
David aka joshua aka baihaqi
