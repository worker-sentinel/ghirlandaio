# Install NetworkManager

Install NetworkManager

``` 
sudo pacman -S networkmanager

```
Setelah semua selesai kita akan melakukan akrifkan dan start untuk NetworkManager

```

sudo enable NetworkManager

```

```

sudo systemctl start NetworkManager  

```

Setelah itu Cek Status 

```

systemctl status NetworkManager  

```

Tampilan NetworkManager

<img width="4032" height="3024" alt="hasil - tampilan networkmanager" src="https://github.com/user-attachments/assets/89fc32c8-99fb-4382-82c8-f820cdb4fb55" />


# Install Desktop Plasma

1. Meriksa update terlebih dahulu

<img width="1296" height="1200" alt="image" src="https://github.com/user-attachments/assets/e189d03a-ce87-49cc-b84e-cb9e8d51c975" />

2. Kemudaian Install Plasma

<img width="1600" height="1272" alt="image" src="https://github.com/user-attachments/assets/de2a05fa-6d72-4066-b29f-5663a1eaf233" />

Tampilan setelah selesai

<img width="1599" height="1302" alt="image" src="https://github.com/user-attachments/assets/9e164b48-f296-4045-8475-d6b16533efee" />

3. Lalul setelah selesai, Aktifkan SDDM

<img width="1599" height="1302" alt="image" src="https://github.com/user-attachments/assets/73dc0c54-5610-47ec-9c74-7cf7f4a7ba39" />

Setelah itu start SDDM 

```
sudo systemctl enable sddm.service
```

Tampilan setelah selesai install plasma

<img width="3264" height="2448" alt="IMG_20260519_085045_814" src="https://github.com/user-attachments/assets/cedde5a5-805c-49c0-87ed-5ba07ab8684a" />

<img width="4032" height="3024" alt="IMG_1712" src="https://github.com/user-attachments/assets/b4d1ea1e-430a-4ce1-a5b1-89c44759e65e" />

# Install Auido Pipewire

1. Install Pipewire

```
sudo pacman -S pipewire pipewire-alsa pipewire-jack pipewire-pulse wireplumber 

```

<img width="1597" height="1361" alt="image" src="https://github.com/user-attachments/assets/dacf98f4-608b-4001-951f-cdb49d7c185a" />

2. Setelah installasi selesai, aktifkan dan jalankan layanan Pipewire, PluseAudio, dan WirePlumber

```
systemctl --user enable --now pipewire.service

```

```
systemctl --user enable --now pipewire-pulse.service

```

```
systemctl --user enable --now wireplumber.service

```

<img width="1523" height="1058" alt="image" src="https://github.com/user-attachments/assets/81310a72-af57-4b5a-a32d-20498987782f" />

<img width="4032" height="3024" alt="hasil - tampilan audio_pipeware 1" src="https://github.com/user-attachments/assets/89466e64-a5ae-475f-9d24-472ad1bcd64e" />


# Install File Manager Dolphin

Install Dolphin

```

sudo pacman -S dolphin

```

<img width="1599" height="1302" alt="image" src="https://github.com/user-attachments/assets/848eb30b-5f2b-4370-8332-4c45120529f3" />

Tampilan File Manager Dolphin

<img width="4032" height="3024" alt="IMG_1716" src="https://github.com/user-attachments/assets/111f8ba4-c81b-4ce0-a5fd-805603600032" />

# Install Terminal Kitty

Install Kitty

```

sudo pacman -S kitty

```
<img width="1599" height="1302" alt="image" src="https://github.com/user-attachments/assets/8a0f5d4b-e8cd-4b8f-b2b6-2bd62d24973a" />

Tampilan Terminal Kitty

<img width="4032" height="3024" alt="IMG_1717" src="https://github.com/user-attachments/assets/9bd9be65-ff74-4f57-866c-1b32563b18e0" />
