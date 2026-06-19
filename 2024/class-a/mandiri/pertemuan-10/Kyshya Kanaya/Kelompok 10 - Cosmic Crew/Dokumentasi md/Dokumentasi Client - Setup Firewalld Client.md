# Setup Firewalld

```
ssh kel10@10.119.250.30
```

```
sudo su
```

```
firewall-cmd --permanent --new-zone=wifi2
```

```
firewall-cmd --permanent --zone=wifi2 --add-source=10.10.10.12/24
```

```
firewall-cmd --permanent --zone=wifi2 --add-port=80/tcp
```

```
firewall-cmd --permanent --zone=wifi2 --add-port=443/tcp
```

```
firewall-cmd --list-all-zones
```
Tampilkan semua zone beserta konfigurasinya untuk verifikasi sebelum reload.

```
firewall-cmd --reload
```
Terapkan semua perubahan permanent yang sudah ditambahkan.

```
firewall-cmd --list-all-zones
```
Verifikasi ulang semua zone setelah reload, pastikan zone wifi2 sudah aktif dengan konfigurasi yang benar.

```
exit
```
