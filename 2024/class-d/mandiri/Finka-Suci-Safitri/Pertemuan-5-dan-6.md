# Cara Install Arch Linux
## Nama: Finka Suci Safitri
## Kelas: 4D
## NIM:12402051030091
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

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/4da39ea3-7c71-4c9a-ac3e-f94c2a309d27" />


Step 6: 
- lsblk

Step 7: 
- cfdisk /dev/sda

Step 8: delete semua

Step 9: ‘new’

Step 10: 
1G untuk boot type efi system,
4G untuk type linux swap,
14G untuk type linux filesystem (memo)

Step 11: 
- pencet 'write’
- yes
- quit
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/f7be5b51-27b7-4ce2-9886-0264bdc923ff" />


Step 12: 
- lsblk

<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/b90976e9-f8f0-45e0-891c-dc37d1bc4f99" />


Step 13: 
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
swapon /dev/sda2

Step 16: 
format boot
- mkfs.fat -F 32 /dev/sda1
<img width="1527" height="1250" alt="image" src="https://github.com/user-attachments/assets/37ea03ff-5f3d-4df9-91f0-edb6ceb5d6f7" />


Step 17:
mount partisi
- mount /dev/sda3 /mnt

Step 18: 
- mount --mkdir /dev/sda1 /mnt/boot

Step 19: 
- pacstrap -K /mnt base-devel linux linux-firmware nvim intel(sesuai laptop masing)-ucode

download
<img width="1600" height="828" alt="image" src="https://github.com/user-attachments/assets/869c1b91-a704-42eb-bda3-e79807257cd4" />


Step 20:
generate fstab
- genfstab -U /mnt >> /mnt/etc/fstab

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/1da1019e-2e92-44b7-b824-23998494d5eb" />


Step 21: 
masuk ke sistem arch
- arch-chroot /mnt

Step 22: 
mengatur timezone
- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

Step 23: 
sinkronkan dengan hardware clock
- hwclock --systohc

<img width="1517" height="797" alt="image" src="https://github.com/user-attachments/assets/65717fe5-5c50-4e9a-b6da-3edfb2c56d44" />


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

enter sampai pilihannya y/n, tulis ‘y’
 
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

<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/a34d3eb2-8062-482a-ad4b-991ae0ed062d" />


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

Dokumentasi Penginstall-an Arch Linux

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/10808e59-3433-4672-b4eb-4c5a08ab88ff" />


<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/cca58c72-4f0e-4c3b-9be7-144b68d487e1" />

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/718fd499-6251-4c7f-872a-baf804f90537" />

<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/b9869dc9-c88d-4afa-b9fe-e7e4554d5716" />

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/53d58136-9da7-4862-916c-6cfdf10ddbd6" />

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/0fce3d57-9e3b-49ad-bb64-8e7964c84ca8" />




