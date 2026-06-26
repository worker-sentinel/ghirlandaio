# Setup Firewalld

### 1. Lihat seluruh zona

```
firewall-cmd --list-all-zones
```

### 2. Izinkan akses ke port 8080

```
firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="10.10.1.253/24" port port="8080" protocol="tcp" accept'
```

### 3. Izinkan akses SSH (port 22)

```
firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="192.168.2.11" port port="22" protocol="tcp" accept'
```

### 4. Reload konfigurasi

```
firewall-cmd --reload
```

### 5. Verifikasi aturan

```
firewall-cmd --list-all-zones
```
```
exit
```

# Pengecekan
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/d3c6d3ef-2491-4dfb-8e25-ec9d7af2a700" />
