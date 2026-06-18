# Networking-Conf-Operator

# Create Operator User Connection

```
nmcli connection add type ethernet ifname [interface] con-name "operator-connection" ipv4.method manual ipv4.addresses [ip address for operator]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

# Automated Login Script

```
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```

```
exit
```

---
- Untuk mengecek [interface], [ip address for operator]/[CIDR], dan [ip address gateway] maka gunakan command ```ip a,``` lalu isi sesuai dengan hasil yang muncul
- [operator] diganti dengan nama nama operator yang sudah dibuat [contoh: rangers]

Rekaman asciinema: https://asciinema.org/a/ANkmNEZjYLnn17Sk
