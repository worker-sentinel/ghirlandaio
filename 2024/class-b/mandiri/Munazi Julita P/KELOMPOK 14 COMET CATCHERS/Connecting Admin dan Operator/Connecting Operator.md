# Dokumentasi Connecting Operator

```
nmcli device status
```

```
sudo nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4 gateway 10.10.10.1 ipv4.dns 8.8.8.8
```

Masukkan password for operator:......

```
nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4. gateway 10.10.10.1 ipv4.dns 8.8.8.8
```

```
nmcli connection up operator-connection

ip a

ip r

ping 8.8.8.8

exit
```
