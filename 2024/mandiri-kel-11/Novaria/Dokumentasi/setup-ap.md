# masuk terlebih dahulu ke directory /var/lib/iwd
```
cd /var/lib/iwd
```
# setelah masuk ke directory iwd, buat folder ap nya
```
mkdir ap
```
# setelah buat, masuk kedalam directory ap
```
cd ap
```
# setelah masuk, kita buat file untuk ap
```
nvim [namafile.ap]
```
# lalu isi
```
[General]
Enable=true
SSID=[namafile]
[Security]
Passphrase=[pw]
[IPv4]
Address=[isi bebas ip addressnya]
Netmask=255.255.255.0
```

# setelah isi 
ketik
```
sudo systemctl restart iwd
```
```
iwctl
```
```
device list
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile [namafile]
```
# cek di device masing-masing

