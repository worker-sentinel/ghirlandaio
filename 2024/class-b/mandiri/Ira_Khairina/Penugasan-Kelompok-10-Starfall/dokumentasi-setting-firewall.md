#### 1. Cek zona-zona yang ada 

```bash
Firewall-cmd --list-all-zone
```

Jika tidak muncul zona, maka beri izin dan mulai firewalld dulu
```bash
Systemctl enable firewalld 
Systemctl start firewalld
```
#### 2. Menuju ke zona spesifik

Lalu ulang command pertama, nanti akan muncul zone-zone yang tersedia.
Jika ingin menuju ke zona yang spesiifik, maka gunakan command berikut. Kali ini kami akan menuju zona publik 

```bash
firewall-cmd --info-zone=public || (ini masuk ke public)
```

#### 3. Hapus service yang tidak terpakai

Hapus service yang tidak dipakai karena rawan menjadi celah keamanan.
```bash
	‘firewall-cmd --zone=public --remove-service=dhcpd6-client --permanent’ || Hapus semua selain ssh.
```

#### 4. Cek apa service sudah terhapus 

```bash
firewall-cmd --info-zone=public || cek dulu
```

Jika masih ada, reload firewall
```bash
firewall-cmd --reload 
```

Cek lag
```bashi 
firewall-cmd --info-zone=public
```

#### 5. Cek Zona lainnya

Lakukan langkah yang sama dengan zona lainnya yang tersedia. Setelah ini akan mengecek zona home

```bash
firewall-cmd --info-zone=home
```

Jika servicenya banyak dan ada lebih dari satu sert akan dihapus bersamaan, maka gunakan tanda kurung kurawal seperti berikut: 
```bash
firewall-cmd --zone=home --remove-service={dhcpd6-client,mdns,samba-client} --permanent
```
Ikuti cara yang sama untuk zona yang lainnya, seperti internal, eksternal, dmz, dll. Lalu reload firewall.
