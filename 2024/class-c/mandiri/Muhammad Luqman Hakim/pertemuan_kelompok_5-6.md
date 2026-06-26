```
cfdisk
```
| partisi  | kapasitas |
| ---------|-----------|
| boot     |   1.5G    |
| swap     |   4G      |  
| root     |   35G     |

## setelah bagi partisi
connect wifi
```
iwctl
```
```
stattion wlan0 connect namawifi
````
cek partisi
```
lsblk
```
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/79e4851d-263e-4e8f-8ef9-e135085d55ba" />

format root, swap, boot
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/66aa0cce-1104-4c24-846a-82079b733e7b" />

# instal sistem dasar
```
pacstrap -K /mnt base linux linux-firmware base base-devel networkmanager
```
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/29c50b54-ad7a-494e-bc69-c85079227c45" />

# Masuk ke chroot
```
arch-chroot /mnt
```

# mengatur timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

# localization
```
pacman -S neovim
```
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/54f046fc-6c6b-4660-b675-e5126358abfa" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/50f8036a-41af-4072-914b-2afce421aa3c" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/c5eb7d08-9f9e-410d-b2e3-b37cc9f74ef6" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/39f20003-5a1a-487a-876b-e92337591031" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/78452419-9ede-4f6d-9387-099ec831d70b" />

# instalasi plasma
<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/06117c21-8bbc-4934-89b8-7772dbd2e235" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/94fbc502-4f3e-453a-b23e-773e79b37606" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/23549b21-c7fd-448f-8bf8-45460e9d3efd" />

```
pacman -S plasma sddm pipewire pipewire-pulse dolphin kitty
```

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/138b66bc-ad6b-42fd-8e41-60e09dd6dfec" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/de9d8fab-270b-4072-bdb5-cd91b4135203" />

```
sudo systemctl enable --now sddm
```

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/f55e7fe8-0614-4b44-bf3b-b707f219aa77" />

