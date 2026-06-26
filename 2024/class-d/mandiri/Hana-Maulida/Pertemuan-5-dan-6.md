# Cara Install Arch Linux
## Nama: Hana Maulida
## Kelas: 4D
## NIM: 12402051050152
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum

Jika sudah terinstall, masukan flashdisk, restart laptop, pilih usb flashdisk, pilih arch linux

Step 1:
- iwctl

Step 2:
- station wlan0 get-networks (untuk sambungin ke wifi)
  
Step 3:
- station wlan0 connect (nama wifi)
  
Step 4: masukin password wifi

Step 5: exit

Step 6:
- lsblk
  
Step 7:
- cfdisk /dev/sda
  
Step 8: delete semua

Step 9: ‘new’

Step 10: 
- 1G untuk boot type efi system 4G untuk swap, type linux swap 14G root, type linux filesystem (memo)

Step 11:
- pencet 'write’
- yes
- quit
  
Step 12:

- lsblk
  
Step 13: format root ke ext4
- mkfs.ext4 /dev/sda3 (nvme0n1p7 jadi sda3)
- tulis ‘y’
  
Step 14: format partisi tambahan

- mkfs.ext4 /dev/sda2
- tulis ‘y’
  
Step 15: aktifkan swap jika cadangan ram penuh

- mkswap /dev/sda2 swapon /dev/sda2
  
Step 16: format boot

- mkfs.fat -F 32 /dev/sda1
  
Step 17: mount partisi

- mount /dev/sda3 /mnt
  
Step 18:

- mount --mkdir /dev/sda1 /mnt/boot
  
Step 19:

- pacstrap -K /mnt base-devel linux linux-firmware nvim intel(sesuai laptop masing)-ucode
download

Step 20: generate fstab

- genfstab -U /mnt >> /mnt/etc/fstab
  
Step 21: masuk ke sistem arch

- arch-chroot /mnt
  
Step 22: mengatur timezone

- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
  
Step 23: sinkronkan dengan hardware clock

- hwclock --systohc
  
Step 24:

- locale-gen
- nvim /etc/locale.conf
- pencet i
- LANG=en_US.UTF-8
- pencet esc
- :wq
  
Step 25:

- nvim /etc/isi nama kita sbg host
- pencet i
  
Step 26:

- mkinitcpio -P
  
Step 27:

- nvim /etc/locale.gen cari #en_US. UTF dan #en_US.ISO hapus pagernya (tambahin huruf i didepan nya supaya bisa hapus pagernya)
- pencet esc
- :wq
  
Step 28:

- useradd -m -G wheel -s /bin/bash (nama user)
- passwd (user)
- bikin password
  
Step 29:

- nvim visudo /etc/sudoers.d/administrator
- pencet i
- user ALL=(ALL:ALL)ALL
- pencet esc
- :wq
  
Step 30:

- pacman -S kitty dolphin sddm networkmanager plasma pipewire
- enter sampai pilihannya y/n, tulis ‘y’

Step 31:

- systemctl enable NetworkManager.service
- systemctl enable sddm.service
  
Step 32:

- pacman -S efibootmgr grub os-prober
- tulis ‘y’
  
Step 33:

- grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=GRUB
  
Step 34:

- grub-mkconfig -o /boot/grub/grub.cfg
  
Step 35:

- exit
  
Step 36:

- umount -R /mnt
  
Step 37:

- reboot
- lepas flashdisk
  
Step 38:

- login arch linux
- masukin password user
  
Selesai

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/a0cf855f-12ff-41f1-9087-feb53b2e39ed" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/41327de0-3d15-4d38-a891-552680134e48" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/294110cd-8a49-4f22-b318-79a4449329b8" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/160c0608-184d-4ea2-9a96-52fc2eb32fc6" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/d9650317-b7b0-4e33-8082-982a611c8d15" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/9a02c636-406d-4b4c-bc38-c1ec810a33c1" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/fe8401a8-cc5f-4251-b7e4-250f54c8e2b8" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/6d47dc19-7594-4e43-9b2f-9e69a40c3423" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/d1536cdf-2f01-4930-b361-b44352e5b2cf" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/b4aa6de3-c190-43d9-a08c-37a85a9f4e4e" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/4a598f41-c8b6-4492-81d7-c0995eb60ad2" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/f9e31f78-4030-4d74-b7a2-56d391040a38" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/3c15fbd8-6f69-41f0-a94b-d42c6fed7d5c" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/597e8432-4803-4a41-a64a-b00db61c6a55" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/69afa04c-abf7-40a3-8287-2a9bcaa101af" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/34070412-6301-4b61-a80c-663f62de616b" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/731be80d-9487-4ed4-bb68-7903e4f50d9f" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/d0cad933-1464-4b0f-a806-2f20e3d2a7e2" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/cf57b040-1864-4921-ace8-ade9504ba0d9" />















