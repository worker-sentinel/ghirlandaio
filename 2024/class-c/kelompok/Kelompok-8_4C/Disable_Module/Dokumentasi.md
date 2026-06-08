# DOKUMENTASI DISABLE MODULE KERNEL

## Konfigurasi Jaringan
```
nvim /etc/iwd/main.conf
```
> Biarkan tetap sepertu itu, lalu esc :wq
```
systemctl restart iwd
```
```
iwctl
```
Connect Wifi
```
station wlan0 get-networks
```
```
station wlan0 connect (nama wifi)
```
```
exit
```
```
ping 8.8.8.8
```

---
## Blacklist modul kernel via Modprobe
```
nvim /etc/modprobe.d/01-hardening.conf
```
> Biarkan seperti itu, lalu esc :wq

Mencopot Modul yang sedang aktif secara Instan
```
modprobe -r usb-storage 2>/dev/null
```
```
modprobe -r bluetooth 2>/dev/null
```
Memperbarui Ramdisk Kernel
```
mkinitcpio -P
```
Pre-check status module
```
lsmod | grep usb-storage
```
```
lsmod | grep bluetooth
```

---
## Hardening Jaringan Berstandar CIS
Konfigurasi Parameter Kernel
```
nvim /etc/sysctl.d/99-cis.conf
```
Buat seperti ini
```
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.log_martians = 1
kernel.randomize_va_space = 2
kernel.sysrq = o
kernel.dmesg_restrict = 1
fs.suid_dumpable = 0
net.ipv6.conf.all.disable_ipv6 = 1
```
Instalasi Tools dan Penerapan Konfigurasi Sysctl
```
pacman -S procps-ng
```
```
sysctl -p /etc/sysctl.d/99-cis.conf
```
Pengecekan hasil Sysctl
```
sysctl net.ipv4.ip_forward
```
```
sysctl kernel.randomize_va_space
```
```
sysctl kernel.sysrq
```

---
## Konfigurasi Firewall Hardening
Mengaktifkan Firewall
```
systemctl enable --now firewalld
```
Set Default Zone to Drop
```
firewall-cmd --set-default-zone=drop
```
Membuka akses untuk layanan web
```
firewall-cmd --zone=drop --add-service=http --permanent
```
Cek alamat IP
```
ip addr show wlan0
```
Mengizinkan koneksi dari jaringan sendiri
```
firewall-cmd --zone=drop --add-source=(alamat IP sendiri) --permanent
```
> ubah angka terakhir sebelum tanda garis miring menjadi 0.
> contoh : alamat Ip 10.252.10.248/24, maka menjadi 10.252.10.0/24.
```
firewall-cmd --reload
```
```
firewall-cmd --list-all
```
