# Explanations
firewalld D nya itu ` Daemon `
dikembangkan oleh rethad menggunakna nftables. mendukung ipv4 dan ipv6, ethernet bridge dan IP set
whitelist = defaultnya tidak ada akses yang di blokir, satu satu di buka atau dizinkan
blacklist = sebaliknya
TCP = protokol transfer data
UDP = protokol transfer data 

### untuk melihat informasi zona
```
firewall-cmd --list-all-zones
```

gunakan ini kalau ingin melihat daftar lengkap seluruh zona beserta aturan (port yang dibuka,layanan yang dizinkan) dimasing-masing zona

### untuk melihat informasi zona secara spesifik 
```
firewall-cmd --info-zone=[nama_zona]
```
ganti nama_zona dengan zona yang mau kamu lihat contohnya: public, home, work

### untuk mengubah zona default
```
firewall-cmd --get-default-zone
```

```
firewall-cmd --set-default-zone= [zone]
```


## service

### untuk melihat list service 
```
firewall-cmd --get-services
```

### untuk melihat informasi service

```
firewall-cmd --info-service [service_name]
```

### untuk menambahkan service ke zone tertentu

```
firewall-cmd --zone=[zone_name] --add-service [service_name]
```

### untuk menghapus service di zone tertentu

```
firewall-cmd --zone=[zone_name] --remove-service [service_name]
```
## ports

### menambahkan port di zona tertentu
```
firewall-cmd --zone= [zone_name] --add-port [port_num] / [protocol]
```

### menghapus port di zona tertentu 

```
firewall-cmd --zone= [zone_name] --remove-port [port_num] / [protocol]
```



# Ip adress
batas IP yang bisa di pake untuk user biasa itu dari 0 sampai 254 
kalau udah 255 itu ip broadcast
broadcast = jenis ip address yang berfungsi untuk mengirim mengirimkan data ke semua host dalam 1 jaringan
hostaddress= IP yang digunakan di 1 perangkat contoh = 192.168.0.1
networkaddrass = IP yang merepresentasikan sebuah alamat network = 192.168.0.0 
gateaway= 10.10.1.1
kelas IP kalau kelas A dari 0000-127.255.255.255(jaringan besar) 1 provinsi atau negara
kelas B dari 128.0.0.0-191.255.255.255 (kelas menengah) contoh 1 gedung gede 
kelas C dari 192.0.0.0-223.225.225.255 (kelas bawah) contoh 1 ruangan

DHCP= alokasi IP yang otomatis atau dia yang membagikan 
# Subneting

