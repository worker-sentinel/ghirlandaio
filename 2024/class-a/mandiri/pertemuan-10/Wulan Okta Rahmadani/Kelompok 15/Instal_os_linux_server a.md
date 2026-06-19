## document installasi OS blackbird-amanda dengan CIS implementation

> hiraukan tanda // tanda // hanya sebuah bentuk comment untuk penjelasan mengapa harus begini dan fungsinya apa <br>
> focus pada isi [      ]  berisi informasi yang di butuhkan untuk istruksi <br>
> focus pada tanda ( G| M ) sebagai pembeda besaran isi partisi volume gruop <br> 
> focus ! tanda untuk penjelasan fungsi dari 

isntallasi linux bllackbird-amanda-os dengan implementasi CIS 

## nyalakan asciinema 

asciinema rec [namavideomu].cast 

## nyalakan internet 

iwctl  // untuk masuk ke iwd <br>

cari card wifi kalian <br>

device list // untuk me list device yang ada, setelah ada dan fungsinya on bisa kalian lanjutlkan tetapi jika beluk kalian bisa lihat pada [https://wiki.archlinux.org/title/Iwd](url) untuk configurasinya <br> 

station wlan0 scan // biasanya device menggunakan wlan0, scan berfungsi untuk menscanning wifi yang ada <br>

station wlan0 get-network // untuk mendapatkan network <br>

station wlan0 connect "namanetwork" , masukan pharaphrashe; paswordnya ke parapharse <br>

check network dengan ping 1.1.1.1  // 1.1.1.1 ping menuju ke dns nya <brr> 

## membuat partisi untuk CIS 

cryptsetup luksFormat /dev/[partisimu]  // cryptsetup untuk mengunci dan membuat encrypsi pada partisi tersebut , menggunakan partisi yang kamu pilih 

buka partisi dengan -- cryptsetup luksOpen /dev/[partisi] [nama-device-yang-ingin-dibuat] // membukan dan menetapkan nama untuk device tersebut  <br>

pvcreate // membuat psychal volume pada device <br>
vgcreate // volumegroup untuk device <br>
lvcreate // logical volume pada volumegroup <br>

pvcreate /dev/mapper/[namadevice-yang-telah-dibuat] <br>
vgcreate [namavolumegrup-yang-ingin-dibuat] /dev/mapper/[namadevice-yang-ingin-dibuat] // membuat volumegroup dari psychal vlomue device<br>

## cara check untuk partisi crypsetupmu
pvscan // untuk scan psycal volume <br>
vgscan // untuk menscan volumegruop <br>
lvscan // untuk scan logical volume yang aktif <br>

## membuat logical volume pada volume gruop 
// untuk size atau besaran isi volume menyesuaikan dari besaran partisi 

lvcreate -L ( G | M ) [namavolumegruop] -n root <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vars <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vtmp <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vlog <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vaud <br>
lvcreate -L ( G | M ) [namavolumegruop] -n home <br>
lvcreate -l100%FREE  [namavolumegruop] -n podman  // podman sedikit berbeda karena ini untuk server <br>

## membuat format 
mkfs.vfat  -F 32 -n BOOT /dev/[partisibuatmu] <br>
mkfs.ext4 -b4096 /dev/[namavolumgroup] /root <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vars <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vtmp <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vlog <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vaud <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /home <br> 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /podman <br> 

## mounting 
mount /dev/[namavolumegroup]/root /mnt <br>
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi-boot] /mnt/boot <br>
mount --mkdir -o rw,nosuid,nodev,relatime /dev/[namavolumegroup]/var /mnt/var <br>
mount --mkdir -o rw,nosuid,nodev,,noexec,relatime /dev/[namavolumegroup]/vtmp /mnt/var/tmp <br>
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/vlog /mnt/var/log <br>
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/vaud /mnt/var/log/audit <br> 
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/home /mnt/home <br>
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/podman /mnt/var/lib/containers <br> 

! //  mkdir itu fungsinya untuk membuat directory dari root,vars,etc <br> 
! // root dan vars tidak menggunakan option noexec agar system dapat membaca root dan vars dan mengeksekusinya <br>

## pacstrap 
pacstrap // fungsinya untuk mengclone packages repository dari Arch karena balckbird-amanda berbasis Arch dan memasukannya ke dalam ISO sebelum menggunakan dan masuk dalam environment arch-chroot <br> 

! // gunakan [amd-ucode] untuk amd 
! // gunakan [intel-ucode] untuk intel 

pacstrap /mnt pacstrap /mnt base intel-ucode linux-lt linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman docker

! // tidak mengunakan meta base atau base-devel , sepenuhnya menggunakan seleksi berdasarkan keperluan user dalam hal ini bisa di seleksi lewat [https://archlinux.org/packages/core/any/base/](url) user dapat mencari dan memillah yang di butuhkan 

## copy configurasi network dari ISO ke arch-chroot 
cp /etc/systemd/network/* /mnt/etc/systemd/network <br> 

##fstab 

genfstab -U /mnt >> /mnt/etc/fstab      // untuk mengenerate mounting pada fstab ketika file menyiapkan initramfs, dapat menentukan outomatis file mana yang akan di mounting atau di gunakan lalau menyiapkannya pada bootloader untuk menggunakan initramfs 

 echo "tmpfs /tap tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab  /tmpfs <br>
! // untuk temporay seperti chache agar langsung menggunakan ram dan dapat memproses percepatan mounting juga penghapusan chache secara automatis, diharapkan tidak salah dalam penulisan 

## arch-chroot 
masuk ke arch-chroot /mnt 

## set hostname 
echo "namalaptopkamu" >> /etc/hostname <br> 
cat /etc/hostname <br>

## set time zone 
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime <br>
hwclock --systhoc <br>

## localegen 
nvim etc/locale.gen <br>
!// uncomenting tanda # pada en_US contoh : #en_US <br> 
locale-gen <br>
locale >> /etc/locale.conf <br>

## useradd 
useradd -m [namausermu] <br>
passwd [namausermu] <br>
echo "[namausermu] ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/terserahkamunamanyaapa <br>

## membuat paramenter untuk initramfs atau kernel parameters 
mkdir /etc/cmdline.d  //untuk membuat direktori untuk kernel parameternya <br>
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}  // memasukan kernel parameter ke sini <br>
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisirootkamu])=nama [devicemu] root=/dev/[volumegroup]/root" > /etc/cmdline.d/01-boot.conf <br>

echo "rw" > /etc/cmdline.d/02-misc.conf<br> 

## configurasi mkinitcpio

##menghapus intiramfs-linux-lts.img 

ls /boot <br> 
rm /boot/initramfs-linux-lts.img <br> 

! //menghapus initramfs lama yang tidak di perlukan <br> 

nvim /etc/mkinitcpio.conf <br>
! // cari hooks yang tidak memiliki comment# lalumasukan sd-encrypt lvm2 setelah sd-vconsole <br> 

nvim /etc/mkinitcpio.d/linux-lts-preset <br>

! // uncomenting #  ALL_config, ALL_kver,ALL_kerneldest <br>
! // commenting # default_config <br>
! // uncomennting # default_uki , hapus /efi/ yang kecil lalu ganti dengan /boot/ <br> 

## membpersipkan looader 
// bootloader menggunakan systemd-boot  <br> 
! // lakukan dengan ,   bootctl --path=/boot install  <br> 

## load dan generate / siapkan iniramfs 
mkinitcpio -P 

## enable service 
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 

# selesaikan record asciinema 
ctrl + d atau exit <br> 
upload asciinema [namarecordvideokamu].cast 

## tahap akhir 
umount -R /mnt <br> 

reboot <br>
