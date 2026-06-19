# Set up admin ke server

## Login ke Server

```
ssh <nama_server>@<ip_server>
```

## Membuat Zona Firewall Admin
```
sudo firewall-cmd --permanent --new-zone=admin
```

## Menambah IP ke Admin
```
sudo firewall-cmd --permanent --zone=admin --add-source=<buat_ip_admin>
```

## Mengizinkan akses SSH ke Admin
```
sudo firewall-cmd --permanent --zone=admin --add-service=ssh
```

## Membuka akses zone Port 
```
sudo firewall-cmd --permanent --zone=admin --add-port=3306/tcp

sudo firewall-cmd --permanent --zone=admin --add-port=6379/tcp

sudo firewall-cmd --permanent --zone=admin --add-port=80/tcp

sudo firewall-cmd --permanent --zone=admin --add-port=443/tcp
```

# referensi
https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-b/mandiri/Kaafi_Alfath_Syahri/Tugas_kelompok_10_Supernova/configurasi_firewalld_atmin.md?plain=1
