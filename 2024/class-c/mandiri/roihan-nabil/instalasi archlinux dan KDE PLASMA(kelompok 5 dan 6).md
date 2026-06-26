# Instalasi Archlinux
---

## 1. Masuk ke live environtment archlinux

#### BIOS HP
Pencet esc sampai masuk BIOS, terus F9 untuk ke BOOT MENU.. pilih yg flashdisk

---
<img width="4064" height="3048" alt="1  masuk ke arch live environment dgn pencet esc berkali kali" src="https://github.com/user-attachments/assets/686b3947-07bd-4862-9876-1f4119cfa21a" />

---

## 2. Jaringan

```iwctl``` dulu, terus ketik perintah ```station wlan0 connect "nama wifi"``` buat nyambungin. Kalau sudah, tinggal masukin password-nya aja. Ketik ```exit``` kalau sudah dan cek internet dgn ```ping 8.8.8.8```

---
<img width="4064" height="3048" alt="2  Iwctl trus masuk ke wifi dgn command station wlan0 connect (nama wifi), stelah itu masukkan password" src="https://github.com/user-attachments/assets/a75a1015-4527-4ce5-9d10-3822da8be103" />

---


## 3. Cek Internet dan Sinkronisasi Waktu

Ketik ```exit``` kalau sudah dan cek internet dgn ```ping 8.8.8.8``` dan memasukkan timedatectl

---
<img width="3631" height="1162" alt="3  tes internet dengan command ping 8 8 8 8 dan memasukkan timedatectl(sinkronisasi waktu)" src="https://github.com/user-attachments/assets/c27b3726-4be9-4fcf-8de1-54776c06a201" />

---


## 4 Partisi (format root, membuat swap dan juga aktivasi swap)

```lsblk``` untuk melihat partisi, ketik ```cfdisk _dev_nvme0n1``` untuk format partisi

---
<img width="4064" height="3048" alt="4  lsblk untuk melihat partisi, ketik cfdisk _dev_nvme0n1 untuk format partisi" src="https://github.com/user-attachments/assets/a689eb6a-55c8-483b-9851-ce9d8e6f9b9a" />

---


Input partisi nvme0n1p5 512M(EFI BOOT), p6 4G(Swap), p7 45G(Root). setelah itu format partisi root ```mkfs.ext4 /dev/nvme0n1p7```, membuat swap ```mkswap /dev/nvme0n1p6```dan aktivasi ```swapon /dev/nvme0n1p6```

---
<img width="4064" height="3048" alt="5  input partisi 0n1p5 512M(EFI BOOT), p6 4G(Swap), p7 45G(Root)  setelah itu format partisi root _mkfs_, membuat swap dan aktivasi swap" src="https://github.com/user-attachments/assets/6bb1aa73-b8fb-410d-afa4-4bdf07bd224e" />

---

## 5 Format EFI(Boot)

```mkfs.fat -F 32 /dev/efi_system_partition``` 

Untuk Mount partisi Root, ketik:

```mount /dev/root_partition /mnt```

Untuk Mount partisi EFI, ketik:

```mount --mkdir /dev/efi_system_partition /mnt/boot```

---
<img width="4064" height="3048" alt="6  format EFI stelah itu mount root dan EFI" src="https://github.com/user-attachments/assets/8b577bcc-6f37-4099-8bc8-50bf3e612ac2" />

---

## 6. Instalasi sistem dasar base linux linux-firmware base devel networkmanager

---
<img width="4064" height="3048" alt="7  instalasi sistem dasar base linux linux-firmware base devel networkmanager" src="https://github.com/user-attachments/assets/55c41043-3ffe-4fb3-afd2-51fd39d22226" />

---

## 7. Membuat fstab, masuk ke sistem chroot, mengatur timezone dan hardware clock


---
<img width="4064" height="3048" alt="8  membuat fstab, masuk ke sistem chroot, mengatur timezone dan hardware clock" src="https://github.com/user-attachments/assets/8db0482d-88e7-4aec-bd1c-0e27363af4ee" />

---


buat file fstab dengan mengetik:

```genfstab -U /mnt >> /mnt/etc/fstab```


Selanjutnya ketik :

```arch-chroot /mnt```

Mengatur Timezone :

```ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```

Untuk mengsinkronkan Hardware Clock ketik :

```hwclock --systohc```


## 8. Localization


Instalasi NEOVIM dulu

```pacman -S neovim```

lalu :

``` nvim /etc/locale.gen```

---
<img width="1440" height="2560" alt="9  (localization) Setelah menginstall neovim lgsung saja menghapus # di en_US UTF dan ISO  lalu escape dan ketik _wq" src="https://github.com/user-attachments/assets/b4497471-a807-41c1-a479-4bd30650b79d" />

---

Setelah menginstall neovim lgsung saja menghapus # di en_US UTF dan ISO  lalu tekan escape dan ketik ```_wq```

Dan di lanjutkan dengan :

```locale > /etc/locale.conf```

---
<img width="1600" height="1200" alt="10  menghapus yg paling atas yg huruf _C_ menjadi en_ slanjutnya yg bagian paling bawah juga setelah LC_ALL=en_US UTF-8 " src="https://github.com/user-attachments/assets/8ed827cf-0f80-438c-9ec4-1a440482901d" />

---
Menghapus yg paling atas yg huruf _C_ menjadi en_ slanjutnya yg bagian paling bawah juga setelah LC_ALL=en_US UTF-8


---

---
<img width="3048" height="4064" alt="11  ini yg uda diinput_" src="https://github.com/user-attachments/assets/4db6a097-9636-4505-95f3-d4b379b71718" />

---

ini yg uda diinput


```nvim /etc/locale.conf```


Setelah sudah terisi, tekan key escape pada keyboard dan ketik ```:wq```


## 9. Hostname, passwd root, bootloader

untuk membuat Useradd, ketik:

```useradd -m (nama user)``` 

utk password root :

```passwd (nama user)```

Masuk Mode Root :

```echo "(nama user) ALL=(ALL:ALL) ALL " > /etc/sudoers.d/00_namauser```

TERUS INSTALL BOOTLOADER(GRUB)

```pacman -S grub efibootmgr os-prober```

```grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB```

```grub-mkconfig -o /boot/grub/grub.cfg```

---
<img width="4064" height="3048" alt="12  input hostname, root password  setelah itu masuk mode root dan install bootloader(GRUB)" src="https://github.com/user-attachments/assets/d611980b-c810-4a5e-b1c3-125c82263223" />

---

##  10. Reboot

Ketik ```Exit```

Selanjutnya, lakukan Unmount dengan mengetik:

```umount -R /mnt```

Lalu ```reboot```

Penting :

Pastikan USB installer/Flashdisk telah dilepas setelah melakukan proses restart.

---
DONEEE

---

# Instalasi KDE PLASMA
---

## 1. Masuk ke boot awal lagi (error dikit)

Karna saya salah dlm masukin password root dan bagian sudoersnya, jadinya saya tidak bisa login didalam archlinuxnya  jadi saya kembali ke arch live environtmen dan memasukkan kembali usb flashdisk saya

---
<img width="3048" height="4064" alt="1  karna saya salah dlm masukin password root dan bagian sudoersnya, jadinya saya tidak bisa login didalam archlinuxnya  jadi saya kembali ke arch live environtmen dan memasukkan kembali usb flashdisk saya" src="https://github.com/user-attachments/assets/14af99d9-f535-462d-9665-1b1b87818ecf" />

---

Tidak memulai dri awal banget, saya hanya mount root, EFI, dan aktivasi Swap. Setelah itu masuk ke archchroot dan instalasi plasma

---
<img width="3048" height="4064" alt="2  tidak memulai dri awal banget, saya hanya mount root, EFI, dan aktivasi Swap  Setelah itu masuk ke archchroot dan instalasi plasma" src="https://github.com/user-attachments/assets/003f6cb1-d0f8-4385-a8f0-8540df00b18f" />

---

## 2. Memulai instalasi plasma

Dengan cara :

```pacman -S dolphin kitty konsole pipewire```

***dolphin(utk file-manager), kitty(utk terminal), konsole(jg utk terminal), pipewire(utk audio)

---
<img width="3602" height="1230" alt="3  instalasi plasma dengan symtax networkmanager(utk internet), pipewire(audio), dolphin(file-manager), kitty_neovim_konsole(utk terminal)" src="https://github.com/user-attachments/assets/123012ff-e3ad-4874-84b3-d2e82f2ae4ab" />

---

## 3. Selesai install KDE PLASMA

Masukkan systemctl enable dan networkmanager (seperti di foto). setelah itu umount dan reboot(jangan lupa cabut usb)

---
<img width="3679" height="761" alt="4  masukkan systemctl enable dan networkmanager  setelah itu umount dan reboot(jangan lupa cabut usb)" src="https://github.com/user-attachments/assets/fdc68411-97da-4be9-9c75-2a013eed49c0" />

---

## 4. Masuk ke LoginScreen

Masukkan Password user kalian

---
<img width="4064" height="3048" alt="5  masukkan passwd user_" src="https://github.com/user-attachments/assets/9d0c99f5-4641-4d6a-91eb-aa7234776caf" />

---
<img width="4064" height="3048" alt="6  selesai dan tampilan desktop akan seperti ini" src="https://github.com/user-attachments/assets/3a70bd66-066c-482a-bc2c-e2ef26e88787" />

---

***tampilan desktop akan seperti ini***

## 5. Jgn lupa untuk masuk ke wifi agar bisa melakukan instalasi software

---
<img width="3048" height="4064" alt="7  jgn lupa untuk masuk ke wifi agar bisa melakukan instalasi software, dll" src="https://github.com/user-attachments/assets/bc6e491b-845e-49c9-a95d-2ea9fd4c753a" />

---


## 6. Ini saya mencoba multilib(biar bisa baca 32bit) dgn cara install nano dlu baru masukkan syntaxnya dan install firefox

--
<img width="4064" height="3048" alt="8  ini saya mencoba multilib(biar bisa baca 32bit) dgn cara install nano dlu baru masukkan syntaxnya dan install firefox_" src="https://github.com/user-attachments/assets/0f5207fc-6b17-4904-acd6-5d506dd9029b" />

--

## 7. Firefox bisa di run, dan cek audio apakah sudah aman, dolphin manager, juga kitty terminal

---
<img width="4064" height="3048" alt="9  firefox bisa di run, dan cek audio apakah sudah aman" src="https://github.com/user-attachments/assets/5393f1e9-a6c3-48bb-aca8-151dd4c16483" />

---

firefox

---
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-23 at 5 41 22 PM" src="https://github.com/user-attachments/assets/2f57ee0c-5955-47bf-b8cc-36eac79f22e6" />

---

audio

---
<img width="3048" height="4064" alt="10  dolphin(filemanager) aman" src="https://github.com/user-attachments/assets/c63eea37-89dc-48a0-9f0d-930c9eab5122" />

---

filemanager

---
<img width="4064" height="3048" alt="11  kitty(terminal) aman, ping aman" src="https://github.com/user-attachments/assets/6db9e717-d8af-43d2-a9d7-c57d4955ca98" />

---

kitty terminal

*** Selesai***
---




