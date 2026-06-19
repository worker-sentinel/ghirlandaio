## Memeriksa Status Layanan SSH
```
systemctl status sshd
```
## Melakukan Koneksi SSH ke Server Tujuan
```
ssh kel10@192.168.2.110
```
*Output*
```
kel10@192.168.2.110's password:
```
## Verifikasi Login Berhasil
```
[kel10@supernova ~]$
```
## Instalasi Podman Compose
```
sudo pacman -S podman-compose
```
*Output*
```
[sudo] password for kel10: warning: podman-compose-1.6.0-1 is up to date -- reinstalling
Packages (1) podman-compose-1.6.0-1
:: Proceed with installation? [Y/n]
```
## Membatalkan Instalasi Ulang
```
n
```
## Logout dari Server Remote
*Perintah*
```
logout
```
*output*
```
Connection to 192.168.2.110 closed.
```
