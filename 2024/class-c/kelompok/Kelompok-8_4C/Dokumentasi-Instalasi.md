# Dokumentasi Instalasi OS Blackbird

---
## CONNECT WIFI
Sambungkan koneksi wifi
```
iwctl
```

cek wifi
```
device list
```

Menyambungkan wifi
```
station wlan0 connect (nama wifi)
```

```
exit
```

Periksa koneksi apa sudah tersambung
```
ping 8.8.8.8
```

---
## MEMBUAT PARTISI
Pembagian Partisi
```
cfdisk /dev/(partisi)
```

Minimal partisi disesuaikan dengan penyimpanan
```
boot = 4G (EFI system)
root = 44.8G (Linux filesystem)
```


