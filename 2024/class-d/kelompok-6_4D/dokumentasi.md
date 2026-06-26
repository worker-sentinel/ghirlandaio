## setelah masuk Arch install dari BIOS 

## masukan internet / koneksi internet
__iwctl__ <br>
untuk masuk ke iwd 
__device list__  <br> 
untuk melihat list device
 __station [nama_device] scan__ <br>
 untuk scan 
__station [nama device] get-networks__ <br>
untuk mengambil dari scan
__station [nama_device] connect__ <br>
untuk koneksi ke wifi 
__station [nama_device} show__ <br> 
untuk melihat status 
exit  <br>

## lihat partisi 
 __fdisk -l__ <br>
 untuk melihat partisi <br>
__lsblk__ <br>
fungsi yang sama tetapi lebih minim dan fungsi melihat mana yang di block 

## membuat partisi 

__cfdisk__ <br>
untuk mengubah partisi secara keseluruhan <br>
cfdisk ke partisi contoh __/dev/sda__ or __/dev/nvme0n1__ <br>

cfdisk /dev/nvmeon1 <br>
cfdisk /dev/sda <br>

contoh tabel untuk bentukan partisinya 
| sda/nvme0n1 | format type | mount point | G |
|:------------|:------------|:------------|:---|
|sda1/nvme0n1p1    | mkfs.fat    | /mnt/boot   | 1G |
|sda2/nvme0n1p2    | mkswap      | swapon      | 4G |
|sda3/nvme0n1p3    | mkfs.ext4   | /mnt        | 50G |

## membuat format 

 __mkfs.fat -F 32__
 untuk membuat format efi boot loader <br>
contoh  /dev/sda1 or /dev/nvme0n1p1 <br>
mkfs.fat -F 32 /dev/nvme0n1p1 <br>
mkfs.fat -F 32 /dev/sda1 <br> 

pilih sesuai type kalian apakah nvme atau sda 
__mkkswap__
untuk membuat format swap <br>
contoh /dev/sda2 or /dev/nvme0n1p2 <br> 
mkswap /dev/nvme0n1p2 <br>
mkswap /dev/sda2 <br> 

pilih sesuai type kalian apakah nvme atau sda 
__mkfs.ext4__
 untuk membuat format root 
 contoh sda/3 or /dev/nvme0n1p3 <br>
 mkfs.ext4 /sda3 <br>
 mkfs.ext4 /nvme0n1p4 <br>  
ikuti sesuai type kalian apakah sda atau nvme0n1

## me mounting point 
### mounting partisi bentuk nvme0n1
__mount /dev/nvme0n1p3 /mnt__ <br>
untuk me mount partisi root alias /mnt <br> 
__mount --mkdir /dev/nvme0n1p1 /mnt/boot__ <br>
untuk me mount partisi ke boot nantinya <br> 
__swapon /dev/nvme0n1p2__  <br>
untuk mengaktifkan fungsi swap <br>

begitu juga tinggal di ganti saja bagi sda jadi sda1 , sda2 , sda3 , sda 4 

setelah format dan mounting berhasil ketik 

## pacstrap
pacstrap -K /mnt intel-ucode base base-devel linux linux-firmware git neovim iwd <br> 
pacstrap -K /mnt amd-ucode base base-devel linux linux-firmware git neovim iwd <br> 

pakai intel untuk intel, pakai amd untuk amd , ini di tentukan prosessor kalian, ini service, entential pacages dan berisi base dari Arch-Linux 

linux-firmware itu berisi driver yang membantu dalam OS linux begitu pula base-devel dan mirror-code kalian yang amd atau intel itu 

## fstab
fstab -U /mnt >> /mnt/etc/fstab <br> 
fungsinya untuk menyiapkkan file yang sudah di mount tadi <br> 

## arch-crhoot /mnt 
masuk ke root dengan mengetik ini <br> 
arch-chroot /mnt <br> 

## menyesuaikan waktu 
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime <br> 
hwclock --systohc <br> 

ini untuk menyesuaikan waktu dan jam agar tersinkronisasi

## locale gen
nvim /etc/locale.gen

#/en_US cari ini dengan ketik /

unmount # dari en_US dengan cara tekan i untuk insert dan :wq untuk write quite 

locale > /etc/locale.conf

ganti lang c menjadi en_US.UTF-8 
di paling bawah yang ALL=  juga begitu ganti seperti di atas 

## useradd groupadd /etc/sudoers.d/

useradd command untuk menambah user 
groupadd command untum menambah additional group ( default group menggunakan wheel) 
/etc/sudoers.d/ adalah tempat menyimpan user yang memiliki izin suo 

cara menambahkan <br>
useradd -m -G wheel -s /bin/bash user <br>
passwd untuk mengamankan izin user <br>
passwd user <br>
untuk memasang izin pengaman pada user <br> 

EDITOR=nvim visudo /etc/sudoers.d/administrator <br>

masukan user ALL=(ALL:ALL) ALL <br>

## install DE dekstop environment dan etentital dependencies or packages 
__sudo pacman -S plasma kitty dolphin pipewire networkmanager sddm__ 

__systemctl enable NetworkManager.service__ <br>
ini untuk wifi

__systemctl enable sddm.service__ <br> 
ini untuk login 

## untuk os prober untuk dual boot windows 

__nvim /etc/default/grub__

uncoment GRUB_DISABLE_OS_PROBER=false 

mount ke /boot kemudian jalankan  grub-mkconfig  

nqnti di lanjutin saat grub-install 

## sudo
adalah bentuk izin pada sistem, sedangkan pacman berfungsi untuk mengambil dependencies atau packages dari mirror yang ada pada arch linux atau repo milik arch linux itu sendiri 

## GRUB efi bootloader

cd boot/

__sudo pacman -S efibootmgr grub os-prober__

efibootmgr menyediakan tampilan environment boot dari grub, grub itu semacam alat untuk menjalankan booting dan os prober itu alat untuk mendeteksi windows

__nvim /etc/default/grub__

__grub-install --target=x86_64-efi --efi-directory=/boot --efi-bootloader-id=Arch-Linux__ 

grub-mkconfig -o /boot/grub/grub.cfg

## mkinitcpio -P 

gunanya untuk mengenerate image uefi bootloader dari linux untuk linux  

exit ke root biasa 

kemudian umount -R /mnt 

kemudian reboot dan cepat copok flashdisk setelah layar menghitam 

semoga berhasil
