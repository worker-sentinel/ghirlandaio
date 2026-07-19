# installasi k3s
## mengecek ip a
```
ip a
```
## mengaktifkan sinkronasi waktu 
```
sudo timedatectl set-ntp true
```
## menginstall k3s
```
sudo curl -sfl https://get.k3s.io | sh -s server
```
## melihat status layanan k3s
```
sudo systemctl status k3s.service
```
## kembali ke user 
```
sudo su
```
## pindah direktori
```
cd /var/lib/
```
## masuk ke direktori server k3s
```
cd /var/lib/rancher/k3s/server/
```
## menampilkan isi direktori k3s
```
ls
```
## menampilkan kode token
```
cat node-token
```
## lalu cek ip 
```
ip a
```
## mengecek status firewalld
```
sudo systemctl status firewalld
```
## membuka port 6443 pada firewall
```
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```
## menetapkan perubahan konfigurasi firewall
```
sudo firewall-cmd --reload
```
## menanmpilkan daftar nodeb pada cluster k3s
```
sudo kubectl get node
```
## kembali ke root
```
exit
```
