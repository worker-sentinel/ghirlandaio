### Bersihkan Service Firewall 


### Masuk ke root

```
sudo su
```

### Cek status Firewall
```
systemctl status firewalld
```

### Melihat zona-zona yang ada
```
firewall-cmd –list-all-zone
```

### Menghapus service yang tidak diperlukan

**Misal zona work**
```
firewall-cmd –zone=work –remove-service={dhcpv4-client,ssh} --permanent
```
**hanya mengganti zona dan service yang ada dizona, lalu sisakan service ssh di zona public**

### Reload
```
firewall-cmd --reload
```

### Cek ulang zona
```
firewall-cmd –list-all-zone
```
**atau jika ingin mengecek zona yang baru dihapus servicenya saja bisa menggunakan ini**
```
firewall-cmd --info-zone=work
```

**SELESAI**
