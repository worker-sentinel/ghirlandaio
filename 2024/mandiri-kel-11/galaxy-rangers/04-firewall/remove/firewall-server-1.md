# Mengecek Status Firewall
```
sudo systemctl status firewalld
```
# Mengecek Zona Firewall
```
sudo firewalld-cmd --list-all-zone 
```
# Remove Zona
```
sudo firewall-cmd --zone=[zone yg mau diremove] --remove-service={banyaknya service yang mau dihapus} --permanent 
```
# Reload
```
sudo firewall-cmd --reload
```
# Cek Zona dengan Spesifik
```
sudo firewall-cmd --info-zone=work (mengecek zone dengan spesifik) 
```
---
# CATATAN 
1. Semua zona tidak boleh ada services nya kecuali zona public. Services di zona public hanya ada ssh saja
2. Tanda kurung kurawal ( {} ) digunakan kalau yang mau diremove lebih dari satu
