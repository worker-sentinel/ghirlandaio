# Cara Install Arch Linux
## Nama: Cinta Husnadzan Dzikri
## Kelas: 4D
## NIM: 12402051050150
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
1G untuk boot type efi system
4G untuk swap, type linux swap
14G root, type linux filesystem (memo)

Step 11: 
- pencet 'write’
- yes
- quit

Step 12: 
- lsblk

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

<img width="1280" height="720" alt="a838ff40-3293-4adb-b114-254df56352cb" src="https://github.com/user-attachments/assets/9634340f-a122-43df-b97f-d226e2f3d41f" />

<img width="1280" height="720" alt="f3ec56a2-b667-4608-938c-24c831c2f6d4" src="https://github.com/user-attachments/assets/c442d97c-b1f8-4d39-95e0-a8ae1f45440d" />

<img width="1600" height="1200" alt="76b8b0ff-484d-4f3d-b2cd-b9fa3d66f3e5" src="https://github.com/user-attachments/assets/929d0098-672e-4781-9f82-a71adcfa1f3a" />

<img width="1200" height="1600" alt="1ce2ed84-1811-40d4-a70e-d7b99e62f2d0" src="https://github.com/user-attachments/assets/eeab0731-0e54-47e7-be9b-318ac189447a" />

<img width="1200" height="1600" alt="4d80e09c-119c-4219-a926-adea09988039" src="https://github.com/user-attachments/assets/a0f46717-0373-4092-8a69-beb0ce50cdf0" />

<img width="1200" height="1600" alt="50597583-b7cc-48d0-9a53-f0fab94b81be" src="https://github.com/user-attachments/assets/91969bbf-3dbe-47f9-814f-09e25652f22c" />

<img width="1200" height="1600" alt="a54d6511-f8d5-46fd-9ffa-34881e24ba04" src="https://github.com/user-attachments/assets/124d48d5-a9f7-48f3-b654-99bf7b9d3744" />
