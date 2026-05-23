Anggota Kelompok 6 : 
1. Marsha Dwi Della
2. Artika
3. Naila Nur Luna

# Menginstall desktop arch linux

## 1. Mengaktifkan Layanan Jaringan Internet 

### Ketik perintah berikut untuk masuk  ke konsol interaktif:

```
iwctl
```

```
[iwd]# device list
```

```
[iwd]# station wlan0 scan
```

```
[iwd]# station wlan0 get-networks
```
```
[iwd]# station wlan0 connect "Nama_WiFi_Pengguna"
``` 

### Verifikasi Koneksi : 
```
ping -c 3 google.com
```
<img width="889" height="657" alt="image" src="https://github.com/user-attachments/assets/429b4f41-4894-489b-b8d8-58de0669c42d" />


## 2. Membuat Pengguna Baru (user) 
```
(useradd -m -G wheel -s /bin/bash kelompok6)
```

## 3. Memberikan proteksi sandi akun 
```
passwd kelompok6
```
<img width="800" height="604" alt="image" src="https://github.com/user-attachments/assets/0ab378e0-be28-4306-8c4b-918a77b1118d" />

## 4. Install Semua Package 
```
pacman -S networkmanager plasma sddm pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber dolphin kitty
```
<img width="474" height="669" alt="image" src="https://github.com/user-attachments/assets/b045beff-0713-462a-b2bc-be93e99fee6d" />


### Mengaktifkan Layanan Jaringan
```
systemctl enable NetworkManager
```
### Aktivasi Layar Login Grafis (SDDM)
```
systemctl enable sddm
```

## 5. Hak Akses Administrator Sudo
```
EDITOR=nvim visudo
```
```
ketik % wheel ALL=(ALL:ALL) ALL
```
<img width="597" height="657" alt="image" src="https://github.com/user-attachments/assets/855a927e-aec0-4eb1-bbd5-46e105fd9099" />


## 6. Membangun ulang (rebuild) citra ramdisk awal untuk semua kernel Linux yang terpasang

```
mkinitcpio -P
```

## 7. Finalisasi & Siklus Reboot
```
exit
```
```
umount -R /mnt
```
<img width="1110" height="608" alt="image" src="https://github.com/user-attachments/assets/bc34d15b-9a31-4fcc-8af8-4bffe265ecbd" />


<img width="1123" height="628" alt="image" src="https://github.com/user-attachments/assets/d654cbe5-7f9d-413a-b908-a1c40bf5f464" />

<img width="1133" height="632" alt="image" src="https://github.com/user-attachments/assets/b2acfb47-3f1b-4fc8-9dee-1a19c72ae332" />

<img width="1133" height="622" alt="image" src="https://github.com/user-attachments/assets/c0335716-7df3-4938-a529-7ba86da96e70" />








