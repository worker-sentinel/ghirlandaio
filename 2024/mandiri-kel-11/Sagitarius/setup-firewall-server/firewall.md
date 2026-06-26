# Firewall server1

## Mengizinkan IP Laptop Admin mengakses port
```
sudo firewall-cmd --zone=public --add-ric-rich-rule='rule family="ipv4" source address=(ip admin) port port="22" protocol='tcp' accept'
```
setelah itu ada tulisan sukses kalian bisa lanjut ketik

## Untuk memuat ulang konfigurasi firewall
```
sudo firewall -cmd --reload
```
Setelah itu akan ada tulisan sukses lagi

# Firewall server2

## Mengaktifkan service networkmanager
```
systemctl enable NetworkManager
```
## Untuk menjalan service NetworkManager
```
systenctl strart Manager
```
## Menambhakan firewall agar bisa di akses port 80
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address=(ip admin) port port="22" protocol='tcp' accept'
```
## Untuk memuat ulang konfigurasi firewall
```
sudo firewall -cmd --reload
```
Setelah itu akan ada tulisan sukses lagi
