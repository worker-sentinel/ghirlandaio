# Binding

Di tahap ini setiap server akan dilakukan binding menggunakan google kubernetes engine.

## 1. Download k3s 

```bash
sudo curl -sfL https://get.k3s.io | sh -s server
```

Setelah berhasil diunduh, lakukan pengecekan status k3s

```bash
sudo systemctl status k3s.service
```

## 2. Buat Port 

Masuk ke root

```bash 
sudo su
```

Masuk ke direktori /var/lib 

```bash 
cd /var/lib
```

Cek file yang tersedia

```bash
ls 
```

Dapatkan ip address, pastikan menggunakan koneksi yang sama

```bash
Ip a
```



Cek status Firewall

```bash
  sudo systemctl status firewalld
```

Tambahkan port di zona publik

```bash
Sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```

Lakukan reload firewall

```bash
sudo firewall-cmd --reload
```

Membuat atau mendapatkan token

```bash
sudo kubectl get node
```

## Di Admin

```bash 
ssh nama@ip
```

## 1. Menginstal k3s

```bash
sudo curl -sfL https://get.k3s.io | K3S_URL=”https://ip.:6443” K3S_TOKEN=”token” sh -s - agent 
```

Cek status

```bash 
sudo systemctl status k3s-agent.service
```



Jika tidak aktif, lakukan restart

```bash 
Sudo systemctl restart k3s-agent.service
```

Lalu cek lagi

```bash
sudo systemctl status k3s-agent.service
```

Lakukan langkah yang sama dengan region atau laptop lain 



