# Dokumentasi admin dan client

---
## Membuka Networkmanager TUI
```
nmtui
```
> edit a conncetion 
> activate a connection
> set system hostname
> radio
> quit
```
```
## Pilih nama wifi yang sudah dibuat oleh server 2 (nginx)
```
```
## Menghubungkan wifi menggunakan nmcli
```
nmcli device wifi connect "nama wifi" password "ketik password wifi"
```
## Terhubung
```
jika terhubung muncul Device 'wlp59s0' successfully activated with 'fcaf616e-8878-4c72-9446-1862a071b78f'
```
## Cek apakah sudah terhubung wifi
```
ping 8.8.8.8
```
## Berpindah direktori kerja ke folder /var/lib/iwd
```
cd /var/lib/iwd/
```
## Menampilkan daftar file
```
ls
```
## install iwd
```
pacman -S iwd
```
## Menonaktifkan layanan NetworkManager
```
sudo systemctl disable NetworkManager
```
## Menghentikan layanan NetworkManager
```
sudo systemctl stop NetworkManager  
```
## Mengaktifkan layanan iwd
```
sudo systemctl enable iwd 
```
## Menjalankan layanan iwd
```
sudo systemctl start iwd 
```
## Membuka utilitas iwd
```
iwctl
```
## konek hotspot server nginx
```
station wlan0 connect "nama hotspot"
```
Ketik passwordnya
## Mengedit file konfigurasi hotspot wifi 
```
nvim /var/lib/iwd/(nama wifi).psk 
```
> tambahkan
> [IPv4]
> Adress=192.168.1.10 (mengikuti yang di server 2)
> Netmask= 255.255.255.0 (mengikuti yang di server 2)
> Gateaway=192.168.1.1 (mengikuti yang di server 2)
```
```
## Menambahkan nama host ke alamat ip server
```
nvim /etc/hosts
```
> ganti ip di debit.local.test menjadi ip yang di server 2 untuk client   
> kemudian di debit local di ganti dengan nama hotspot
```
```
## untuk menguji web server dapat diakses melalui nama host
```
curl -v http://contoh.local  
```
## keluar dari terminal
```
exit
```
