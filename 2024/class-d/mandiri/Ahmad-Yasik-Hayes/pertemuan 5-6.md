# Cara install arch linux
## Nama : Ahmad yasik hayes
## Kelas : 4D
## Nim : 12402051030087
## Mata Kuliah : Perpustakaan dan Arsip Digital
## Dosen : Al Muhdil Karim
Cara Install Linux

cek windows+R "diskmgnmt.msc" untuk melihat partisi dan memisahkan partisi yang ada terlebih dahulu

intall rufus untuk mempersiapkan ISO didalam flashdisk agar flasdisk bisa dijadikan bootloader.

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
2G untuk boot type efi system,
4G untuk type linux swap,
32G type linux filesystem (memo)
22G untuk home

Step 12: 
- pencet 'write’
- yes
- quit

Step 13: 
- lsblk

Step 14: 
format root ke ext4
- mkfs.ext4 /dev/nvme
- tulis ‘y’

Step 14:
format partisi tambahan
- mkfs.ext4 /dev/nvme0n1p7
- tulis ‘y’

Step 15:
aktifkan swap jika cadangan ram penuh
- mkswap /dev/nvme0n1p6
- swapon /dev/nvme0n1p6

Step 16: 
format boot
- mkfs.fat -F 32 /dev/nvme0n1p5

Step 17:
mount partisi
- mount /dev/nvme0n1p7 /mnt

Step 18: 
- mount --mkdir /dev/nvme0n1p5 /mnt/boot

Step 19: 
- pacstrap -K /mnt base-devel linux linux-firmware nvim AMD(sesuai laptop masing)-ucode

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
- pencet i untuk insert
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
