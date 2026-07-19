# Dokumentasi menghubungkan admin ke aplikasi
## Hubungkan Jaringan yang sama di laptop B
```
Station wlan0 scan
Station wlan0 connect "..."
```

## Mengecek ip address sudah tersambung atau tidak di laptop B
```
ip a
```

## kalau belum tersambung masukan manual ip laptop B
```
sudo nvim /etc/hosts

masukkan ip nya
192.xxx.x  slims.test
```

## Cek lagi
```
ip a
```

## kalau sudah baru masuk ke aplikasi nya
```
exit
