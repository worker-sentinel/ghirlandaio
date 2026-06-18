sistem manajemen firewall dinamis yang digunakan pada sistem operasi linux. sistem ini bisa memungkinkan user mengatur keamanan jaringan melalui konsep zona, yang menentukan tingkat  kepercayaan (level of trust) untuk berbagai koneksi dan antarmuka. 

sistem ini memungkin kan untuk mengkonfigurasi pengaturan yang berbeda berdasarkan zona, termasuk menentukan zoan bawaan (predefined zones) atau menetapkan sebuah zona utama (default zone).

Kelebihan utama `firewalld` adalah kemampuannya menerapkan perubahan baru tanpa memutuskan koneksi (traffice) yang sdang berlangsung. 

# Direktori
firewalld mendukung dua direktori konfigurasi:

## konfigurasi default dan cadangan

`/usr/lib/firewalld` berisi konfigurasi default dan cadangna yang disediakan oleh firewalld untuk tipe ICMP, layanan dan zona.

package yang disediakan :
1. paket firewalld tidak boleh di ubah.
2. jika firewalld diubah, perubahan akan hilang jika di update packagenya

> Tipe ICMP, layanan, dan zone tambahan dapat disediakan dengan paket atau dengan membuat file

## konfigurasi spesifik sistem

konfigurasi sistem atau pengguna yang tersimpan di `/etc/firewalld` tersebut dapat dibuat oleh administrator sistem, melalui kostumisasi dengan antaramuka konfigurasi firewalld atau  secara manual. 

> file-file tersebut akan menimpa file konfigurasi default.


# runtime vs permanent

## runtime configuration
 adalah konfigurasi yang saat sedang akrif menahan lalu lintas jaringan dikernel. Jika menambahkan aturan disini, aturannya langsung jalan, tapi akan otomatis hilang jika layanan firewalld dimatikan, direstart atau reload.

## permanent configuration
adalah konfigurasi yang ditulis secaa permannen didalam file sistem. Konfiguasi ini tidak lagnsung aktif saat itu juga, namun akan dibaca da diubah menjadi konfiguasi runtime setiap kali komputer bau nyala atau saat layanan firewalld di reload.

## runtime to permanent
runtime environment juga dapat digunakan unuk membuat pengaturan fireall yang sesuai dengan kebutuhan (permanent).
Setelah selesai dan berfugsi, pengaturan tersebut dapat dimigrasikan bersama runtime ke migrasi permanen. Tersedia di `firewall-config` dan `firewall-cmd`.

perintah firewall-cmd adalah:
```bash
firewall-cmd --runtime-to-permanent
```

> jika aturan tidak berfungsi, cukup dengan melakukan 

```bash
firewalld reload
```

atau

```bash
firewalld restart
```

#  firewalld.conf
berada di `/etc/firewalld` menyediakan konfigurasi dasar untuk firewalld. Jika tidak ada akan menggunakan konfigurasi default.

## zona default

**zona default** digunkan jika string zona kosong digunakan. 

> Karena semual hal yang tidak secara eksplisit terikat pada zona lain akan ditangani oleh zona default

contoh:
```bash
DefaultZone=public
```

## minimal mark
**Minimal Mark** ini menentukan batas angka minimum dari label-label tersebut yang **bebas untuk di gunakan** sesuka hati.

> jika nanti dibutuhkan lebih banyak label untuk keperluan konfigurasi yang kompleks, naikan saja nilai minimum nya

```bash
MinimalMark=100
```

## clean up on exit
konfigurasi yang menentukan apa yang harus dilakukan oleh sistem terhadap aturan-aturan firewall saat firewalld dimatikan  atau dihentikan.

> jika nilainya diatur menjadi `no` atau `else`, maka konfigurasi firewall tidak akan dibersihkan ketika layanan firewalld berhenti

contoh:
```bash
CleanupOnExit=yes
```

## lockdown
adalah fitur pengamanan tingkat lanjut untuk melindungi `firewalld` itu sendiri dari perubahan yang tidak di izinkan.

> jika lockdown di aktifkan, maka akses untuk melakukan perubahan aturan firewall akan dibatasi dan dikunci. 

ia akan membaca whitelist bernama `lockdown-whitelist.xml`

contoh:
```bash
Lockdown=no
```

## IPv6_rpfilter
adalah parameter yang berfungsi untuk melakukan uji filter jalur balik (reverse path filter test) khusus untuk paket jaringan IPv6


> ketika sebuah paket data IPv6 tiba di servermu, firewall akan mengecek jalur balasannya. Jika balasan untuk paket tersebut nantinya akan dikirim keluar melalui antarmuka (_interface_) yang sama persis dengan saat paket itu tiba, maka paket tersebut akan diterima. Namun, jika jalurnya berbeda, paket tersebut akan langsung dibuang (_dropped_).

```bash
IPv6_rpfilter=yes
```

## Individual Calls

berfungsi untuk menginstrukskan `firewalld` agar tidak menggunakan pemanggilan perintah gabungan (combined-restore calls), melainkan mengeksekusi aturan-aturan tersebut satu persatu (individual calls)

Jika diaktifkan bagus untuk debugging 

contoh :
```bash
IndividualCalls=no
```

## Log Denied

adalah parameter yang berfungsi untuk menambahkan aturan pencatatan (logging) tepat sebelum aturan penolakan (reject) dan pembuangan (drop) pada lalu lintas jaringan.

> pilihan nilai yang bisa kamu atur untuk mengatifkan fitur ini adalah `all`, `unicast`, `broadcast`,  `multicast` dan `off`

contoh :
```
LogDenied=off
```
****
# utilitas
untuk mengecek status operasi ada dua cara:

## systemctl

> karena firewalld berjalan sebagai layanan (service daemon) pada sistem linux

command:
```bash
systemctl status firewalld
```
### firewall-cmd 
> adalah utilitas baris perintah utama meilik firewalld sendiri.

command:
```bash
firewall-cmd --state
```

command: 
```bash
firewall-cmd --get-active-zones
```
> akan menampilkan daftar zona yang saat ini bersatus "aktf" beserta antarmuka ata sumber lalu lintas (sources) yang terkait ke zona tersebut


# Zona

zona adalah kumpulan aturan yang dapat diterapkan pada antarmuka tertentu.

>Untuk mendapatkan gambaran umum tentang zona dan antarmuka yang saat ini diterapkan:

```bash
firewall-cmd --get-active-zones
```

zona dapat ditentukan berdasarkan nama dengan car meneruskan `--zone=[zone_name]` parameter. Jika tidak ada zona yang ditentukan, zona default akan digunakan.

## informasi zona

kita dapat melihat daftar semua zona berserta konfigurasi lengkapnya: 

```bash
firewall-cmd --list-all-zones
```

atau hanya zona tertentu:

```bash
firewall-cmd --info-zone= [nama_zona]
```

##  Zona perubahan antarmuka

```bash
firewall-cmd --zone=  [zone] --change-interface= [interface_name]
```

> disana `zone` adalah zona baru yang ingin anda tetapkan antarmuka.



## service

service adalah aturan yang telah dibuat sebelumnya yang sesuai dengan daemon tertentu. Misal `ssh` service ini sesuai dengan `ssh` dan membuka port 22 ketika ditetapkan ke suatu zona.

```bash
firewall-cmd --get-service
```

kita juga dapat menanyakan informasi tentang service tersebut:

```bash
firewall-cmd --info-service _ service_name_
```

### menambah atau menghapus layanan dari suatu zona 

untuk menambakan layanan ke suatu zona:
```bash
firewall-cmd --zone= [zone_name] --add-service [service_name]
```

menghapus layanan: 

```bash
firewall-cmd --zone= [zone_name] --remove-service [service_name]
```

## Ports

Ports can be directly opened on a specific zone.
```bash
firewall-cmd --zone= [zone name] --add-port [port_number] / [protocol]
```

disana `protocol` adalah `tcp` atau `udp`.

> untuk menutup port gunakan `--remove-port` dengan nomor port dan protokol yang sama



