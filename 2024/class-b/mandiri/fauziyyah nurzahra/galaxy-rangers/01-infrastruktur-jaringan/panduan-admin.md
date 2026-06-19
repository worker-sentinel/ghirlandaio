# Setup IP Admin

##  Cek Nama Port Kabel LAN (Ethernet)
```
nmcli device status
```

## Lihat Detail Jaringan Saat Ini
```
ip a
```

## Buat Profil Jaringan dengan IP Tetap
> Sekarang kita akan membuat profil jaringan baru. Gantilah teks di dalam tanda < > dengan konfigurasi jaringanmu yang sebenarnya.
```
nmcli connection add type ethernet ifname <nama_port_lan> con-name "admin-connection" ipv4.method manual ipv4.addresses <ip_admin>/<angka_cidr> ipv4.gateway <ip_gateway> ipv4.dns 8.8.8.8
```
Contoh Penggunaan:
```
nmcli connection add type ethernet ifname enp3s0f3u1 con-name "admin-connection" ipv4.method manual ipv4.addresses 18.81.8.88/24 ipv4.gateway 18.81.8.1 ipv4.dns 8.8.8.8
```

## Nyalakan Jaringan Otomatis Saat Login
> Agar jaringan ini langsung aktif secara otomatis setiap kali pengguna masuk ke sistem (login), kita perlu memasukkan perintah aktivasi ke dalam file profil pengguna.

```
echo "nmcli connection up admin-connection" >> /home/galaxy/.bash_profile  
```
## Keluar
```
exit
```

