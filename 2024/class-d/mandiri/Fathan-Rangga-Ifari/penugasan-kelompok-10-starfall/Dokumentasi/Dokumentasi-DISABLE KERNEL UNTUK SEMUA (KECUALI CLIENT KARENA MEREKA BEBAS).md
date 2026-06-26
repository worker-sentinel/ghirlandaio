DISABLE KERNEL UNTUK SEMUA (KECUALI CLIENT KARENA MEREKA BEBAS)

FIREWALL 
Di internet itu ada berbagai ancaman, sehingga firewall yang akan memblokir mereka. Firewall benar-benar dipakai di internet, router wifi, dll. Di firewall ada beberapa zona; drop, blok, public, dmz, external, internal, home, dll. 
Drop : zona paling ketat. Setiap yang masuk ke sini bakal ditolak. Misal kalau ngeping ke sini maka ping nya bakal ditolak. Misal kek punya yang 6.6.6.6 nya departemen keamanan US
Blok : paketnya masuk tapi keblokir (analogi), datanya masuk tapi nggak diterima. 
Publik : Cuma orang-orang tertentu yang bisa masuk. Misal, di sebuah tempat publik kita cuman ngomong orang-orang yang mereka tahu.
Eksternal : Mirip-mirip publik, tapi kek baru ketemu sekali. Zona yang diluar zona internet. Misal, kita di kantor dan di dalam kantor itu ada 2 ip address dan beda router. Misal, di gedung fah kita kenal 1 orang dari lantai bsa. 
DMZ : demilitarized zone. Zona netral. 
Internal : Deket aja. 
Work : 
Home : Jaringan yang udah kita percaya banget. Misal, teman satu tim
Trusted : Paling percaya banget deh pokoknya. 
Ada list di zone itu service-service yang diizinin. Secara default diblok, namun diberikan izin oleh firewall.

COMMAND 

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

SET UP MODULE KERNEL 

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
