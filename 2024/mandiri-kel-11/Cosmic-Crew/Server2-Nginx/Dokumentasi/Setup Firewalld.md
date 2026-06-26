
## 1. Melihat konfigurasi firewall

```
firewall-cmd --list-all-zones
```

## 2. Menghapus Rich Rule yang lama

Menghapus rule yang mengizinkan akses dari IP `10.10.1.20`.

```
firewall-cmd --zone=public --permanent --remove-rich-rule='rule family="ipv4" source address="10.10.1.20" accept'
```

Menghapus rule SSH dari jaringan `192.168.1.1/24`.

```
firewall-cmd --zone=public --permanent --remove-rich-rule='rule family="ipv4" source address="192.168.1.1/24" port port="22" protocol="tcp" accept'
```

## 3. Menerapkan perubahan

```
firewall-cmd --reload
```

## 4. Memverifikasi konfigurasi

```
firewall-cmd --list-all-zones
```

## 5. Menambahkan Rich Rule baru

Mengizinkan akses ke port **8080** dari jaringan `10.10.1.20/24`.

```
firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="10.10.1.20/24" port port="8080" protocol="tcp" accept'
```

Mengizinkan akses **SSH (port 22)** hanya dari IP `192.168.2.10`.

```
firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="192.168.2.10" port port="22" protocol="tcp" accept'
```

## 6. Memuat ulang konfigurasi

```
firewall-cmd --reload
```


## 7. Verifikasi akhir

```
firewall-cmd --list-all-zones
```


<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/170a8b75-cb71-4b3b-9c88-e2e92f31b23d" />

