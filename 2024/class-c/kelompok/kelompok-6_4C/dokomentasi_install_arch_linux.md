# Cara Install Plasma
Jika sudah masuk ke dalam base linux, ikuti langkah-langkah di bawah ini:

### Formating
``` 
root
```
Masukin password

Cek connect wifi
```
iwctl
```
```
station wlan0 connect (nama wifi)
```
(abis itu dicek, udah ter-connect wifi belum)
```
exit
```
```
ping 8.8.8.8
```
## Instal KDE Plasma 
```
pacman -S plasma sddm networkmanager pipewire dolphin kitty
```
jika terjadi eror, seperti : 
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/c28a04bf-171d-4011-9b9a-7c433047ce02" />
maka ketik;
```
systemctl restart systemd - resolved
```
```
systemctl restart iwd
```
lalu connect wifi lagi 

kalau sudah, instal lagi KDE plasma

jika sudah terinstal,
masukin usseradd 
```
usseradd -m  (nama usser)
```
```
password (nama usser) (masukin passwordnya)
```

lalu masuk ke KDE plasma
```
sudo systemctl enable --now sddm
```
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/64fca410-3726-4210-b6db-23962afb77b4" />

# Penggunaan Plasma
## Cara Aktifkan NetworkManager 
buka terminal, lalu ketik 
```
su -
```
lalu masukin password root
```
systemctl start NetworkManager
```
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/8786dcaf-21dc-4103-b971-a47c6139c892" />

## Cara Aktifkan Audio (Pipewire)
Buka terminal
```
sudo pacman -S sof-firmware alsa-firmware alsa-uncm-conf plasma-pa
```
masukin password sudo
```
systemctl --user enable --now pipewire.service
```
```
systemctl --user enable --now pipewire-pulse.service
```
```
systemctl --user enable --now wireplumber.service
```
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/2713db29-9b33-4ecf-8017-69088dfe61f9" />

## Cara Instal WPS
Buka terminal
```
sudo pacman -S flatpak
```
```
flatpak install flathub com.wps.Office
```
## Cara Instal Firefox
```
sudo pacman -S flatpak
```
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
```
flatpak run org.mozilla.firefox
```
```
flatpak update
