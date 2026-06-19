*Install Slims*
## Memeriksa Isi Direktori Home
```
ls
```

## Navigasi ke Direktori Konfigurasi Container
```
cd .config/container/
ls
```
```
cd compose/
ls
```

## Menjalankan Container SLiMS
```
podman compose up -d
```

## Verifikasi Jaringan (IP Address)
```
ip a
```

## Tes connect Internet
```
ping 8.8.8.8
```

## Verifikasi Jaringan (IP Address)
```
ip a
```

## Status Container
```
podman ps -a
```

## Membuka port firewall
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```

Lalu reload firewall
```
firewall-cmd --reload
ip a
```
