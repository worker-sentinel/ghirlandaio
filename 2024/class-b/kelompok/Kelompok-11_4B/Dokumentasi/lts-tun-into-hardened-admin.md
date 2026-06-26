## Merubah Linux lts Menjadi Linux Hardened

### 1. Masuk ke root terlebih dahulu
`sudo su`

### 2. Install linux hardened 
`pacman -S linux-hardened linux-hardened-headers`

### 3. Mengedit file dengan command: 
`nvim /etc/mkinitcpio.d/linux-hardened.preset`

#### Lalu ganti sesuai dengan foto di bawah ini: 
<img width="1280" height="960" alt="WhatsApp Image 2026-06-22 at 10 12 29 PM" src="https://github.com/user-attachments/assets/8d95dc74-9b7b-46d5-91db-d92f86330dcf" />

### 4. Kalau sudah sesuai gunakan command:
`mkinitcpio -P`

### 5. Reboot
