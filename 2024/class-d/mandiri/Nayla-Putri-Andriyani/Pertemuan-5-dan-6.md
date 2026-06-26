# Cara Install Arch Linux
## Nama: Nayla Putri Andriyani
## NIM:12402051030093
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum

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
1G untuk boot type efi system,
4G untuk type linux swap,
14G type linux filesystem (memo)

Step 12: 
- pencet 'write’
- yes
- quit

Step 13: 
- lsblk

Step 14: 
format root ke ext4
- mkfs.ext4 /dev/sda3 (nvme0n1p7 jadi sda3)
- tulis ‘y’

Step 14:
format partisi tambahan
- mkfs.ext4 /dev/sda2
- tulis ‘y’

Step 15:
aktifkan swap jika cadangan ram penuh
- mkswap /dev/sda2
- swapon /dev/sda2

Step 16: 
format boot
- mkfs.fat -F 32 /dev/sda1

Step 17:
mount partisi
- mount /dev/sda3 /mnt

Step 18: 
- mount --mkdir /dev/sda1 /mnt/boot

Step 19: 
- pacstrap -K /mnt base-devel linux linux-firmware nvim intel(sesuai laptop masing)-ucode

download

Step 20:
generate fstab
- genfstab -U /mnt >> /mnt/etc/fstab

Step 21: 
masuk ke sistem arch
- arch-chroot /mnt

Step 22: 
mengatur timezone
- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

Step 23: 
sinkronkan dgn hardware clock
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
- nvim /etc/locale.gen
cari #en_US. UTF dan #en_US.ISO hapus pagernya (tambahin huruf i didepan nya supaya bisa hapus pagernya)
- pencet esc
- :wq

Step 28:
- useradd -m -G wheel -s /bin/bash (nama user yg dimau)
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

enter trus aampe bacaanya pilihannya y/n, tulis ‘y’
 
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

selesai

<img width="720" height="743" alt="image" src="https://github.com/user-attachments/assets/f6adf097-c77c-48a3-bda8-9ba37d63cd1d" />
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/eea7c34a-ab09-448d-810c-79abf629642a" />
<img width="779" height="1040" alt="image" src="https://github.com/user-attachments/assets/59339dfb-3969-4ddb-8d62-947551a65ad3" />
<img width="1040" height="779" alt="image" src="https://github.com/user-attachments/assets/f24e560f-3516-4903-ac7f-92ac3dcc6b25" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/c7226a68-d6f7-40fe-949e-e1e401e1b221" />
<img width="720" height="1280" alt="image" src="https://github.com/user-attachments/assets/91e3d605-aabe-499a-8915-e5b340086543" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/1a9d0e17-ac14-487a-94c1-7af9757e4905" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/4cdd52e6-641f-44c8-9f10-99ad6d5562fc" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/a98aee6b-e02f-4268-8bed-4b39418b336c" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/af53aad8-8fb9-4633-beba-3a6f66f487e7" />
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/b52acb7a-b8bd-4f75-b39f-f4f58de15eb3" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/54d7978b-f042-4f8c-a7b5-61b103c20c4c" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/2e2333e5-7438-40de-8376-c6bee9bb2ce1" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/31c8ab73-3c6a-48b5-84e4-ba01fe84e0c1" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/1e821e91-94d5-4df7-8f24-c60572c1bbde" />







