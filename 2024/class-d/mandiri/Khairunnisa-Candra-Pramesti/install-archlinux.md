# Install Arch Linux
buka situs  https://archlinux.org/download scroll ke bawah sampai ketemu "Indonesia" kemudia klik yang citrahost.com
---
```
 iwctl
```
 <img width="1528" height="1528" alt="wifi" src="https://github.com/user-attachments/assets/71f06ed4-2a35-4ff4-861c-0f7e246bde26" />

 ---
Pembagian partisi
```
cfdisk /dev/nvme0n1
```
<img width="1600" height="900" alt="pembagian partisi" src="https://github.com/user-attachments/assets/fa96c77c-4c95-4115-aa25-9027ca7f3c65" />


| Partisi | Kapasitas |
|---------|-----------|
|  boot   |    1G     |
|  swap   |    4G     |
|  root   |    20G    |

---
fortmat boot, swap dan root
```
<img width="1528" height="1528" alt="efi" src="https://github.com/user-attachments/assets/9179d4a6-2df8-41cc-8c3b-8ed75600d1fe" />
```
Mounting
---
<img width="1528" height="1528" alt="mount" src="https://github.com/user-attachments/assets/79b3f164-d7bc-49cb-a2dd-00cc35eec5f2" />
```

---
## Ins
pacstrap -K /mnt base base-devel linux linux-firmware neovim networkmanager grub efibootmgr os-prober
---

Stelah input timezone lanjutkan dengan locale-gen
```
<img width="1528" height="1528" alt="locale-gen" src="https://github.com/user-attachments/assets/e5d1032e-cb93-45e4-80b0-9f588b0eaaea" />
```

useradd -m khairunnisa
```

<img width="1528" height="1528" alt="useradd -m" src="https://github.com/user-attachments/assets/57bd854e-501e-4de6-94cd-e84f5a7fbe3a" />
```
psswd itu masukin pasword
---
pacman -S neovim

pacman -S plasma sddm pipewire pipewire-pulse dolphin kitty
<img width="900" height="1600" alt="pacman -S" src="https://github.com/user-attachments/assets/b1be449b-3019-41ba-beda-5587c953e549" />
```
## Install Bootloader (GRUB)
<img width="1599" height="899" alt="eror grub-install" src="https://github.com/user-attachments/assets/c341ea7d-36fc-4fbd-823a-cefbc08f559b" />
Pada tahap ini saya mengalami error ketika 
```
grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=arch
```
Alhasil saya tidak bisa
```
exit
```
umount -R /mnt
```
Reboot
---

# Pada tahap install plasma saya bisa meng-install sampai step akhir
```
desktop
<img width="1528" height="1528" alt="plasma=desktop" src="https://github.com/user-attachments/assets/ff3ac915-04d2-4c5f-893c-db92c96489ed" />

---

Aduio
<img width="1528" height="1528" alt="audio" src="https://github.com/user-attachments/assets/320887e2-6703-477a-8df9-6da3db9cc037" />

```
dolphin
<img width="1528" height="1528" alt="file manager = dolphin" src="https://github.com/user-attachments/assets/ea8c6dd5-2203-4f1d-a7a6-f304ab28745b" />

network
<img width="1528" height="1528" alt="network" src="https://github.com/user-attachments/assets/b27f2e43-d542-400b-bc66-cbe882800d1b" />

kitty
<img width="1528" height="1528" alt="kitty = terminal" src="https://github.com/user-attachments/assets/eb85d73d-2f4d-4577-86bd-6e670ef6be1e" />

## Alhamdulillah sudah selseai tetapi masih belum bisa install ulang (error) masih mencari cara install yang baik
