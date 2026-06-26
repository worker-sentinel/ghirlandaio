# Networking-Conf-Operator

##  Cek Informasi Jaringan (Persiapan)
```
ip a
```
## Buat Profil Jaringan Operator
```
nmcli connection add type ethernet ifname <nama_interface> con-name "operator-connection" ipv4.method manual ipv4.addresses <ip_operator>/<angka_cidr> ipv4.gateway <ip_gateway> ipv4.dns 8.8.8.8
```
Contoh Penggunaan:
```
nmcli connection add type ethernet ifname enp3s0f3u1 con-name "operator-connection" ipv4.method manual ipv4.addresses 18.81.8.90/24 ipv4.gateway 18.81.8.1 ipv4.dns 8.8.8.8
```

## Nyalakan Jaringan Otomatis Saat Login

```
echo "nmcli connection up operator-connection" >> /home/<nama_operator>/.bash_profile```

```

## Selesai dan Keluar
```
exit
```

---

