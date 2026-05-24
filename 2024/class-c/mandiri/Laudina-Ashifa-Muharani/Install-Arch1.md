# Dokumentasi Instalasi Arch Linux dan Plasma

## Arch Linux
<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;Arch Linux adalah GNU/Linux serba guna x86-64 yang dikembangkan secara independen distribusi yang cukup serbaguna untuk memenuhi peran apa pun. Arch Linux menggunakan pengelola paket Pacman-nya sendiri, yang memasangkan biner sederhana paket dengan sistem build paket yang mudah digunakan. Singkatnya, Arch Linux adalah distribusi serbaguna dan sederhana yang dirancang untuk sesuai dengan kebutuhan pengguna Linux. 

## Plasma
<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;Plasma adalah tampilan antarmuka pada Linux yang digunakan untuk memebantu pengguna berinteraksi dengan sistem operasi secara grafis. KDE Plasma menyediakan berbagai fitur seperti desktop, taskbar, menu aplikasi, pengaturan sistem, dan widget sehingga pengguna Linux menjadi lebih mudah nyaman. Tampilan KDE Plasma bisa terlihat modern, ringan, dan bisa diubah sesuai kebutuhan pengguna, misalnya mengganti tema, ikon, atau tata letak desktop. Tujuannya untuk kita bisa melihat tampilan beberapa fitur seperti bisa melihat folder, info baterai atau jaringan (wi-fi)

## Instalasi Arch Linux dan Plasma
<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;1. Pada logo Windows, ketik "Data Encryption" dan kemudian buka sampai tampilan berubah menjadi "Device Encryption" dan pastikan dalam keadaan off

---

<img width="1140" height="1022" alt="image" src="https://github.com/user-attachments/assets/00d687ae-cbdb-44fb-96eb-1d1b61fecb1e" /> <img width="606" height="480" alt="image" src="https://github.com/user-attachments/assets/dcc70f22-dec6-4511-8a74-3767338e0fb9" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;2. Unduh Archiso dan rufus

---

<img width="877" height="315" alt="image" src="https://github.com/user-attachments/assets/44ce1aab-0740-4627-9b79-f76c02f758f3" /> <img width="1330" height="1033" alt="image" src="https://github.com/user-attachments/assets/f14b17f3-4182-4473-8fed-acba759197ed" />


---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;3. Tekan Windows + R kemudian ketik cmd. Saat tampilan sudah berubah, ketik diskpart lalu list disk dan pastikan pada kolom GPT terdapat tanda *

---

<img width="267" height="212" alt="image" src="https://github.com/user-attachments/assets/e6d9dc2f-b530-41b6-8d38-26582e97e409" /> <img width="514" height="339" alt="image" src="https://github.com/user-attachments/assets/56672271-473a-43dc-b2e2-ad163f8dcf2a" /> <img width="487" height="577" alt="image" src="https://github.com/user-attachments/assets/67e27e94-0c64-4696-8f26-36aa6b02db87" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;4. Buka Rufus yang sudah diunduh. Pada bagian Boot Selection, ganti dengan Archiso yang sudah diunduh dengan cara menekan select dan pilih Archiso yang sudah diunduh. Ubah Partition Scheme menjadi GPT

---

<img width="448" height="542" alt="image" src="https://github.com/user-attachments/assets/487aa668-b5b9-4638-b66c-73415b16b120" /> <img width="427" height="579" alt="image" src="https://github.com/user-attachments/assets/6b203c16-7876-4367-ae46-735cc46977f3" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;5. Arahkan kursor pada logo windows lalu tekan bagian kanan touch pad sampai muncul pilihan dan pilih Disk Management. Arahkan kursor pada penyimpanan yang paling besar, tekan kanan pada touch pad dan pilih Shrink Volume. Isi bagian "Enter the amount of space in MB" sesuai kebutuhan, tapi disarankan isi 30-50 GB. Berarti, jika 30GB maka isi dengan 30000.

---

<img width="326" height="882" alt="image" src="https://github.com/user-attachments/assets/7b7f8a3a-7e8f-4206-8c39-c142a1c261b5" /> <img width="456" height="574" alt="image" src="https://github.com/user-attachments/assets/7ca2f6c1-eed6-40b6-9d74-19a8b9cf52c5" /> <img width="338" height="495" alt="image" src="https://github.com/user-attachments/assets/84adb754-67ef-495d-9f58-5a24b6ca2c08" /> <img width="354" height="394" alt="image" src="https://github.com/user-attachments/assets/852514b6-44c3-4790-a26f-37178cf07b6b" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;6. Shut down laptop, kemudian nyalakan lagi dengan menekan tombol power lalu tombol F2 dengan cara menekannya berkali-kali sampai muncul tampilan BIOS laptop. Tekan F1, lalu ke Secuity dan pilih Set Supervisor Security dan buat password. Kemudian ke Boot, ke Exit dan pilih Exit Saving Changes. Kemudian laptop akan menampilkan tampilan baru.

---

<img width="474" height="355" alt="image" src="https://github.com/user-attachments/assets/49a24c39-171a-488a-946c-fc7c8a3c7b93" /> <img width="772" height="559" alt="image" src="https://github.com/user-attachments/assets/c4161e57-a4c4-476d-9942-db1084861898" /> <img width="864" height="602" alt="image" src="https://github.com/user-attachments/assets/6b997fb1-acef-475c-a044-c0f8556f1a5a" /> <img width="483" height="340" alt="image" src="https://github.com/user-attachments/assets/7cc67e4e-8724-4ab2-b6bb-38854b10184a" /> <img width="474" height="381" alt="image" src="https://github.com/user-attachments/assets/710bb6f6-8635-4b1a-9298-3976cc342f44" /> 

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;7. Mengecek apakah device sudah siap dengan command cat /sys/firmware/efi/fw_platform_size

---

<img width="821" height="391" alt="image" src="https://github.com/user-attachments/assets/9fbb1c9b-127a-45f1-b8a9-2eef0cc677e8" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;8. Ketik command iwctl untuk perintah connect wi-fi

---

<img width="374" height="322" alt="image" src="https://github.com/user-attachments/assets/ab44e2a0-576d-4e9d-8520-6e96fe840693" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;9. Ketik device list dan cek pada tabel terdapat wlan0 yang artinya nama perangkat serta pastikan pada kolom powered tertulis "on"

---

<img width="344" height="285" alt="image" src="https://github.com/user-attachments/assets/734ff1cf-5fc4-4c67-b504-5151474ecf37" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;10. Ketik command station wlan0 get-networks untuk mencari wi-fi yang ada di sekitar. Jika tidak ada nama wi-fi yang dituju, ketik command station wlan0 scan. Saat sudah tahu mau terhubung ke wi-fi yang mana, ketik command station wlan0 connect (nama wi-fi). Setelah Enter nanti akan diminta untuk mengetikkan password wi-fi yang ingin digunakan. Lalu exit untuk keluar dari menu iwd tersebut.

---

<img width="348" height="526" alt="image" src="https://github.com/user-attachments/assets/5a0450cd-d609-424a-ab75-58caf9315e95" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;11. Untuk mengetahui apakah sudah terhubung dengan wi-fi atau belum, ketik command ping google.com. Lalu tekan ctrl + c untuk menghentikan proses.

---

<img width="347" height="188" alt="image" src="https://github.com/user-attachments/assets/8c66219c-e4e5-462b-a267-b2c1e86f5161" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;12. Lakukan pengecekan partisi dengan mengetik command lsblk -o name,fstype,size. Untuk hasil disesuaikan dengan perangkat masing-masing. Untuk di sini, hasil yang keluar adalah nvme0n1

---

<img width="193" height="81" alt="image" src="https://github.com/user-attachments/assets/562e8a26-d623-475f-a755-7b1c0acf429f" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;13. Ketik command cfdisk /dev/(partisi). Contoh: cfdisk /dev/nvme0n1 lalu akan muncul tampilan berikut

---

<img width="883" height="634" alt="image" src="https://github.com/user-attachments/assets/70ec2b7e-49fd-4924-8d0c-47e3002b2a5f" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;14. Pada tahap ini akan membuat partisi boot, partisi swap, dan partisi root. Pilih Free Space, klik New, ubah ukuran menjadi 1G, ubah Type menjadi Efi System, lalu Enter. Lakukan hal yang sama pada Free Space yang masih ada hasa saja kali ini ubah ukuran menjadi 4G dan Type diubah menjadi Linux Swap. Free Space sisanya tak perlu diubah.

---

<img width="423" height="605" alt="image" src="https://github.com/user-attachments/assets/2187ea8a-4ce1-4185-824a-67711827c81d" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;15. Klik bagian Write dan ketik command lsblk -o name,fstype,size

---

<img width="443" height="311" alt="image" src="https://github.com/user-attachments/assets/89ab7910-20cd-40a7-b1a7-0d583781c9fa" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;16. Buat partisi root dengan command mkfs.ext4 /dev/nvme0n1p. Partisi Efi dengan command mkfs.fat -F 32 /dev/nvme0n1p5. Partisi swap dengan command mkswap /dev/nvme0n1p6 dan untuk mengaktifkannya menggunakan command swapon /dev/nvme0n1p6. Lalu ketik command lsblk -o name,fstype,size

---

<img width="353" height="339" alt="image" src="https://github.com/user-attachments/assets/304c700a-9d4f-4ceb-88dc-f8b3d7928999" /> <img width="443" height="441" alt="image" src="https://github.com/user-attachments/assets/db5dbcc5-6f96-43f9-b374-97f1fe7dc47a" /> <img width="470" height="434" alt="image" src="https://github.com/user-attachments/assets/86f2af95-f911-4669-ab64-685a7bc085fc" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;17. Lakukan mounting dengan command mount /dev/nvme0n1p7 /mnt, lalu ketik command lsblk. Ketik command mount --mkdir /dev/nvme0n1p5 /mnt/boot, lalu ketik command lsblk

---

<img width="490" height="486" alt="image" src="https://github.com/user-attachments/assets/67214b51-b536-4d28-9554-059b881cd525" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;18. Ketik command pacstrap -K /mnt base linux linux-firmware base-devel neovim git networkmanager

---

<img width="495" height="449" alt="image" src="https://github.com/user-attachments/assets/8336c2b9-f11a-4457-8ad8-8f854ab900bd" /> <img width="863" height="555" alt="image" src="https://github.com/user-attachments/assets/8ac60113-f6d5-43fe-a5ee-e1edc3378726" /> <img width="864" height="621" alt="image" src="https://github.com/user-attachments/assets/323985a4-4f75-4cd9-ba09-7f005ab64b83" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;19. Ketik command genfstab -U /mnt > /mnt/etc/fstab dan tekan Enter lalu ketik command arch-chroot /mnt

---

<img width="414" height="64" alt="image" src="https://github.com/user-attachments/assets/8ac454a2-a17c-4e64-8dde-9919d314262e" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;20. Untuk mengatur timezone, ketik command ln -sf /usr/share/zoneinfo/Area/Location /etc/localtime. Contohnya: ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime lalu sinkron dengan hardware clock dengan ketik command hwclock --systohc

---

<img width="375" height="32" alt="image" src="https://github.com/user-attachments/assets/50a2d58a-e017-45d7-929b-5d8032b256ab" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;21. Pada tampilan seperti ini, tekan tombol huruf I yang berarti insert lalu ketik command LANG=en_US.UTF-8
Setelah selesai tekan esc dan ketik :wq


---

<img width="922" height="641" alt="image" src="https://github.com/user-attachments/assets/aecae0ad-3d89-4180-927a-88ef8a7c8799" /> <img width="555" height="622" alt="image" src="https://github.com/user-attachments/assets/78f6628f-e49d-452b-b146-c51248ebea7e" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;22. Ketik command echo "(nama hostname yang mau digunakan)" > /etc/hostname
Contoh: echo "acerlau" > /etc/hostname
Lalu ketik command mkinitcpio -P

---

<img width="825" height="419" alt="image" src="https://github.com/user-attachments/assets/d27a9d52-a7dd-4aa9-b009-609df5a7cf20" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;23. Ketik command useradd -m (user) lalu tekan Enter dan ketik command passwd (user)
Contoh: useradd -m laudina
passwd laudina

---

<img width="347" height="111" alt="image" src="https://github.com/user-attachments/assets/c34160ae-4656-4988-a7b0-7ec36c6c99b3" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;24. ketik command echo '(user) ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none lalu tekan Enter dan ketik command pacman -S grub efibootmgr os-prober
Saat ada pilihan (Y/n) maka ketik Y

---

<img width="620" height="183" alt="image" src="https://github.com/user-attachments/assets/c34d0831-7999-4f7a-a227-bfcfba73d3ef" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;25. Tunggu sampai pengunduhan selesai

---

<img width="893" height="404" alt="image" src="https://github.com/user-attachments/assets/09c79e4c-e906-4427-813d-4171d197ab10" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;26. Ketik command grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
Kemudian ketik command nvim /etc/default/grub

---

<img width="783" height="74" alt="image" src="https://github.com/user-attachments/assets/0009d097-af8e-4c82-b573-18aab394e0a5" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;27. Saat masuk ke tampilan seperti ini, turunkan kursor sampai kalimat akhir dan hapus tanda # pada tulisan #GRUB_DISABLE_OS_PROBER=false

---

<img width="907" height="693" alt="image" src="https://github.com/user-attachments/assets/2eea84d0-6960-4ec7-bac8-de583259f740" /> <img width="814" height="619" alt="image" src="https://github.com/user-attachments/assets/50ab0c9e-d07e-4f9e-baa5-9e1a7c37a167" /> 

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;28. Ketik :wq lalu ketik command grub-mkconfig -o /boot/grub/grub.cfg ketika sudah selesai ketik exit

---

<img width="735" height="475" alt="image" src="https://github.com/user-attachments/assets/f5c2b897-8ec5-459e-907f-ce1f5cd784a9" /> <img width="752" height="160" alt="image" src="https://github.com/user-attachments/assets/868f155e-e028-4903-aa55-450f3f7e8b51" />


---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;29. Ketik command mount -B /mnt lalu tekan Enter lalu kembali ketik command reboot maka nanti tampilan akan berubah. Setelah tampilan berubah, masukkan password yang sudah dibuat saat langkah adduser

---

<img width="752" height="160" alt="image" src="https://github.com/user-attachments/assets/a398ba9a-9e54-432c-bbde-90b6213a69d8" /> <img width="942" height="436" alt="image" src="https://github.com/user-attachments/assets/f9d9ef69-68fe-4e1d-8e6b-e95552cd4530" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;30. Pada tampilan seperti ini, tekan row bawah untuk memilih opsi Activate a connection, pilih wi-fi yang ingin tersambung pada laptop serta masukkan password wi-fi nya dan jika terdapat tanda * pada wi-fi yang dituju maka artinya sudah tersambung

---

<img width="922" height="675" alt="image" src="https://github.com/user-attachments/assets/6d9f7796-0838-4611-bdee-9e7c58bcf63d" /> <img width="870" height="642" alt="image" src="https://github.com/user-attachments/assets/288eb8b7-1fb5-4dd7-befb-ba3d3ec7f76f" /> <img width="883" height="663" alt="image" src="https://github.com/user-attachments/assets/0ac9b18a-cb73-439f-98d4-012a665e7920" /> <img width="446" height="621" alt="image" src="https://github.com/user-attachments/assets/890a7af9-a43b-44e9-8cd5-b9626cec16bb" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;31. Untuk mengeceknya, ketik command ping 1.1.1.1 lalu Enter dan tekan ctrl + c untuk menghentikan proses pengecekan.

---

<img width="946" height="203" alt="image" src="https://github.com/user-attachments/assets/aad1018f-c334-4450-96a2-0fa11888be25" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;32. Ketik command systemctl start systemd-networkd lalu tekan Enter dan ketik command systemctl enable systemd-resolved lalu Enter lagi dan ketik command systemctl start_systemd-resolved

---

<img width="936" height="296" alt="image" src="https://github.com/user-attachments/assets/51f46318-ed29-4040-9470-636fad64ffe5" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;33. Ketik command pacman -S plasma sddm pipewire pipewire-pulse pipewire-jack kitty firefox dolphin dan selanjutnya terus tekan Enter sampai layar belakang berubah menjaid hitam dan ketik Y pada pilihan [Y/n] untuk melakukan pengunduhan

---

<img width="927" height="191" alt="image" src="https://github.com/user-attachments/assets/d8810769-4cbd-484f-973c-128a8fa96d35" /> <img width="913" height="655" alt="image" src="https://github.com/user-attachments/assets/6d89a637-8d0e-4629-8d61-a92ea2163780" /> <img width="934" height="665" alt="image" src="https://github.com/user-attachments/assets/30ab3427-b229-4648-98fe-855e45adbfcc" />


---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;34. Setelah proses unduhan selesai, ketik command systemctl enable sddm kemudian tekan Enter dan ketik command reboot

---

<img width="948" height="682" alt="image" src="https://github.com/user-attachments/assets/a288d896-c76a-456e-9791-7850d7568afe" /> <img width="950" height="566" alt="image" src="https://github.com/user-attachments/assets/6edec281-a961-4b03-83fd-2091d1c1d351" />

---

<p align="justify">
&nbsp;&nbsp;&nbsp;&nbsp;35. Saat tampilan layar berubah maka masukkan password yang sudah dibuat untuk user

---

<img width="854" height="522" alt="image" src="https://github.com/user-attachments/assets/36947977-b14f-4a80-a390-81457d45f91d" /> <img width="907" height="603" alt="image" src="https://github.com/user-attachments/assets/683d5c6d-77d0-4860-873e-e789c23a92e4" />

## Source
https://archlinux.org/about/








































