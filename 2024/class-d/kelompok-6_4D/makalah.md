**<h1 align="center">INSTALASI DESKTOP ARCH LINUX </h1>**

<p align="center"><small>Makalah ini disusun untuk memenuhi tugas mata kuliah Perpustakaan dan Arsip Digital
<p align="center">Dosen Pengampu: Al Muhdil Karim, S.IP, M.Hum.

<p align="center"><img width="690" height="599" alt="690px-logouinsyarifhidayatullahjakarta" src="https://github.com/user-attachments/assets/656b29bd-4701-4b2d-8d57-e0c13afece40" />

<p align="center"><b>Disusun Oleh Kelompok 6 (4D):<b>

<p align="center"><b>1. Achmad Zanuarta Pattisahusiwa (12402051030102)

<p align="center"><b>2. Angel Aisyah Dewi (12402051030095)

<p align="center"><b>3. Resti Irma Agustin (12402051010025)

<p align="center"><b>PROGRAM STUDI ILMU PERPUSTAKAAN</b></p>

<p align="center"><b>FAKULTAS ADAB DAN HUMANIORA</b></p>

<p align="center"><b>UIN SYARIF HIDAYATULLAH JAKARTA</b></p>

<p align="center"><b>TAHUN 2026</b></p>

---

# KATA PENGANTAR

Segala puji dan syukur kami panjatkan kepada Tuhan yang Maha Esa karena berkat karunia-Nya kami mampu menyelesaikan tugas makalah ini dengan baik dan tepat waktu. Makalah yang berjudul “INSTALLASI DEKSTOP ARCH LINUX” disusun guna memenuhi tugas Bapak Al Muhdil Karim, S. IP, M. Hum. Pada mata kuliah Perpustakaan dan Arsip Digital.
Kami menyadari bahwa dalam penulisan makalah ini tidak terlepas dari bantuan banyak pihak, sehingga makalah ini dapat terselesaikan. Kami juga menyadari bahwa makalah ini masih memerlukan penyempurnaan. Oleh karena itu, kami menerima segala bentuk kritik dan saran yang membangun dari berbagai pihak. Apabila terdapat banyak kesalahan pada makalah ini, kami memohon maaf. Demikian yang dapat kami sampaikan. Akhir kata, semoga makalah ini dapat bermanfaat.

# BAB I 
# PENDAHULUAN

## 1.1 LATAR BELAKANG
Arch Linux merupakan distribusi Linux yang dikembangkan secara independen dan bersandar pada arsitektur CPU x86-64 yang umum digunakan. Umumnya, instalasi yang digunakan adalah sistem paling minimal, lalu dikonfigurasikan oleh pengguna sehingga hanya fungsi penting saja yang ada, menurut masing-masing pengguna. Dengan kelima prinsipnya, yaitu kesederhanaan, modernitas, pragmatisme, terpusat pada pengguna, dan keserbagunaan, arch linux menciptakan modifikasi yang minimalis sehingga tidak ada tambahan yang tidak perlu, menjaga versi piranti lunak terbaru tetap stabil, namun harus terhindar dari cacat sistemik, keputusan dibuat berdasarkan kasus-kasus yang dialami oleh para pembuat dan pengurus piranti lunak sebagai panduan, senantiasa selalu menjadi ramah pengguna dan mengisi kebutuhan bagi siapapun yang berkontribusi, serta merupakan distribusi serbaguna, pengguna dapat dengan mudahnya membuat dan mengurus repositorinya sendiri. 

Dekstop Environment (DE) adalah implementasi metafora desktop yang terbuat dari sekumpulan program, yang berbagi Graphical User Interface (GUI) yang sama. Dekstop Environment menggabungkan berbagai komponen untuk menyediakan elemen antarmuka pengguna grafis umum seperti ikon, bilah alat, wallpaper, dan widget desktop. Selain itu, sebagian besar lingkungan desktop menyertakan serangkaian aplikasi dan utilitas terintegrasi. Dalam installasinya terdapat beberapa komponen, seperti Network Manager, KDE Plasma, Pipewire, Dolphin, dan Kitty. 

## 1.2 RUMUSAN MASALAH
1. Bagaimana proses intalasi NetworkManager pada Arch Linux?
2. Bagaimana proses instalasi KDE Plasma pada Arch Linux?
3. Bagaimana proses intalasi Pipewire pada Arch Linux?
4. Bagaimana proses intalasi Dolphin pada Arch Linux?
5. Bagaimana proses intalasi Kitty pada Arch Linux?

## 1.3 Tujuan
1. Untuk mengetahui proses instalasi NetworkManager pada Arch Linux.
2. Untuk mengetahui proses instalasi KDE Plasma pada Arch Linux.
3. Untuk mengetahui proses instalasi Pipewire pada Arch Linux.
4. Untuk mengetahui proses instalasi Dolphin pada Arch Linux.
5. Untuk mengetahui proses instalasi Kitty pada Arch Linux.

# BAB II
# PEMBAHASAN

## 2.1 NetworkManager
NetworkManager merupakan program untuk menyediakan deteksi dan konfigurasi agar sistem terhubung secara otomatis ke jaringan, berguna untuk jaringan nirkabel dan kabel. Untuk jaringan nirkabel, NetworkManager lebih memilih jaringan nirkabel yang dikenal dan memiliki kemampuan untuk beralih ke jaringan yang paling andal. Aplikasi sadar NetworkManager dapat beralih dari mode online dan offline. NetworkManager juga lebih memilih koneksi kabel daripada yang nirkabel, memiliki dukungan untuk koneksi modem dan jenis VPN tertentu.
 
Cara mengaktifkan NetworkManager:
 ```
NetworkManager.service
 ```

 ```
systemctl enable NetworkManager.service
 ```

## 2.2 Plasma
KDE adalah proyek perangkat lunak yang saat ini terdiri dari lingkungan desktop (KDE Plasma). KDE Plasma mengubah tampilan yang hanya berbasis terminal menjadi tampilan dekstop grafis. Merupakan Tampilan Desktop pada linux. Berbeda dengan tampilan dekstop pada windows, KDE plasma bisa mengganti posisi, ukuran dan sampai merubah tata  letak sesuai yang user inginkan.

Command:

 ```
sudo pacman -S plasma
 ```

## 2.3 Pipewire
Pipewire adalah multimedia tingkat rendah yang memiliki fungsi untuk menawarkan pengambilan dan pemutaran untuk audio dan video dengan latensi minimal dan dukungan untuk aplikasi berbasis PulseAudio, JACK, ALSA, dan GStreamer. PipeWire menggunakan model keamanan seperti Polkit, mengharuskan meminta izin Flatpak atau Wayland untuk merekam layar atau audio. 

Command:

 ```
sudo pacman -S pipewire
 ```

## 2.4 Dolphin
Dolphin adalah pengelola file default KDE yang digunakan untuk mengelola file dan folder pada sistem operasi linux, dan dapat memudahkan pengguna dalam melakukan pencarian dan pengelolaan data. Dolphin juga mendukung fitur preview file untuk video, gambar, PDF, audio, dan berbagai format lainnya.

Command:

 ```
sudo pacman -S dolphin
 ```

## 2.5 Kitty
Kitty adalah emulator terminal berbasis OpenGL yang dapat dituliskan dengan TrueColor, dukungan ligatur, ekstensi protokol untuk input keyboard, dan rendering gambar. Ini juga menawarkan kemampuan ubin, seperti GNU Screen atau tmux. Kitty mengizinkan user mengubah atau mengedit sistem operasional linux yang diinginkan menggunakan sudo, mendukung berbagai fitur seperti tab, split window, shortcut keyboard, dan rendering berbasis GPU dengan performa yang cepat dan fitur modern untuk memudahkan pengguna mengoperasikan sistem.

Command:

 ```
sudo pacman -S kitty
 ```

# BAB III
# PENUTUP

## 3.1 Kesimpulan
Arch linux merupakan sistem operasi yang cukup fleksibel mengikuti kebutuhan pengguna. Dengan adanya tambahan komponen pokok seperti Network Manager untuk mendeteksi jaringan internet, KDE Plasma sebagai tampilan dekstrop pada sistem operasional linux, Pipewire untuk mengatur audio , Dolphin berperan sebagai file explorer, dan Kitty sebagai terminal modern.

# DAFTAR PUSTAKA
- https://wiki.archlinux.org/title/Arch_Linux_(Bahasa_Indonesia) 
- https://wiki.archlinux.org/title/NetworkManager 
- https://wiki.archlinux.org/title/KDE#Plasma 
- https://wiki.archlinux.org/title/PipeWire 
- https://wiki.archlinux.org/title/Dolphin 
- https://wiki.archlinux.org/title/Kitty 

---
