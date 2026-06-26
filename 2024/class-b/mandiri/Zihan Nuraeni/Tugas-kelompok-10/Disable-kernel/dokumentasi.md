## Bagian 2 : Disable Kernel 

### 2.1 : Set Firewall 

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

Cek lagi
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

### 2.2 : Set Up Module Kernel

Nonaktifkan module kernel yang tidak dipakai untuk memperkecil celah keamanan. 

#### 1. Buat file konfigurasi 
```bash
	sudo nvim /etc/modprobe.d/01-hard.conf
```

Isi dengan: 
```
install usb-storage /bin/false
blacklist usb-storage
```

Lalu esc :wq

```bash
sudo modprobe -r usb-storage
````

Lalu rebuild initramfs

```bash
mkinitcpio -P
```

Ctrl + d asciinema 
Lakukan reboot untuk semua cluster

```
Ikuti cara yang sama untuk zona yang lainnya, seperti internal, eksternal, dmz, dll. Lalu reload firewall.
