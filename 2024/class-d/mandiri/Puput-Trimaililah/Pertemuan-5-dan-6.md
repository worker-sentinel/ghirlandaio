# Cara Install Arch Linux
## Nama: Puput Trimaililah
## Nim: 12402051010022
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.
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
1G untuk boot type efi system
4G untuk swap, type linux swap
14G root, type linux filesystem (memo)

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
swapon /dev/sda2

Step 16: 
format boot
- mkfs.fat -F 32 /dev/sda1

Step 17:
mount partisi
- mount /dev/sda3 /mnt

Step 18: 
mount --mkdir /dev/sda1 /mnt/boot

Step 19: 
pacstrap -K /mnt base-devel linux linux-firmware nvim intel(sesuai laptop masing)-ucode

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
pencet i

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

<img width="1600" height="1200" alt="860a33c1-6330-4f7e-8efb-4eca85292e84" src="https://github.com/user-attachments/assets/e7575fbe-da12-42d2-a44b-ebcd7da25d43" />
<img width="1600" height="1200" alt="4956e757-7e71-4716-9013-3e1746b7c629" src="https://github.com/user-attachments/assets/e7afb1d0-ad6e-4aa1-9271-ee785c737dda" />
<img width="1600" height="1200" alt="f5be6ce7-36c9-4bc4-a711-e9b9558b47b9" src="https://github.com/user-attachments/assets/7bb45a73-c6b7-45e2-9701-4b7dfe888f60" />
<img width="1600" height="1200" alt="6d7f6a5d-ee81-45b9-9ad7-5d31e2165564" src="https://github.com/user-attachments/assets/4d7f4ecb-33de-4bc0-9466-0f0677346d0c" />
<img width="1600" height="1200" alt="2c364448-b7b4-4c6f-afe6-ddf5946acbeb" src="https://github.com/user-attachments/assets/57aaee13-e25c-40d0-8c76-48fa43514eed" />
<img width="1600" height="1200" alt="3d608f9a-ec3f-4644-91c3-119e66035f2d" src="https://github.com/user-attachments/assets/480781f2-0e65-4b03-9a6e-55754a8c17ae" />
<img width="1600" height="1200" alt="b1c47f80-4c61-413a-a11c-8cb509e40552" src="https://github.com/user-attachments/assets/48ad5f87-3d64-4686-a300-ec3946906a73" />
<img width="1600" height="1200" alt="5536a883-0538-47f8-b779-c7da0e5feb58" src="https://github.com/user-attachments/assets/42cabb7b-8fa4-44d9-8552-277255c53646" />
<img width="1600" height="1200" alt="49423245-9b55-4776-941a-713a90a4fe9d" src="https://github.com/user-attachments/assets/56f708b5-2f0c-4937-af79-de9ccfa73c85" />










