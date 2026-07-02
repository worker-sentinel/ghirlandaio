# Dokumentasi Install Ubuntu VM

---

## 1. Menginstal Paket Virtualisasi

```bash
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

Menginstal paket-paket yang diperlukan untuk membangun host virtualisasi berbasis **KVM/QEMU** di Arch Linux sehingga sistem dapat membuat, menjalankan, dan mengelola virtual machine.

Command `pacman -S` digunakan untuk menginstal paket dari repositori resmi Arch Linux.

Paket yang diinstal meliputi:

- **qemu** : Hypervisor dan emulator yang digunakan untuk menjalankan virtual machine.
- **virt-manager** : Antarmuka grafis (GUI) untuk membuat, mengelola, dan memonitor virtual machine menggunakan libvirt.
- **virt-viewer** : Aplikasi untuk mengakses tampilan (console) virtual machine melalui protokol SPICE atau VNC.
- **dnsmasq** : Menyediakan layanan DNS dan DHCP bagi jaringan virtual yang dibuat oleh libvirt.
- **vde2** : Menyediakan Virtual Distributed Ethernet untuk membangun jaringan virtual.
- **bridge-utils** : Menyediakan utilitas untuk membuat dan mengelola network bridge sehingga virtual machine dapat terhubung langsung ke jaringan fisik.
- **openbsd-netcat** : Utilitas jaringan untuk melakukan komunikasi TCP/UDP, pengujian konektivitas, debugging, maupun transfer data sederhana.

---

## 2. Menginstal Paket Firewall untuk Libvirt

```bash
sudo pacman -S ebtables iptables
```

Menginstal utilitas firewall yang dibutuhkan oleh libvirt untuk melakukan NAT, filtering, dan pengaturan jaringan virtual.
- iptables digunakan untuk melakukan filtering paket jaringan dan NAT.
- ebtables digunakan untuk memfilter trafik pada layer Ethernet (bridge).

## 3. Menginstal Libguestfs

```bash
sudo pacman -S libguestfs
```

Menginstal libguestfs untuk mengakses, memodifikasi, dan memeriksa disk image virtual machine tanpa harus menjalankan VM. </br>
Libguestfs menyediakan berbagai tool seperti guestfish, virt-edit, dan virt-inspector yang berguna untuk administrasi VM.

## 4. Mengaktifkan Layanan Libvirt Saat Boot

```bash
sudo systemctl enable libvirtd.service
```

Mengaktifkan layanan libvirtd agar otomatis berjalan saat sistem boot. </br>
Libvirtd adalah daemon utama yang mengelola virtual machine, storage, dan jaringan virtual.

## 5. Menjalankan Layanan Libvirt

```bash
sudo systemctl start libvirtd.service
```

Menjalankan daemon libvirtd tanpa perlu reboot. </br>
Setelah service berjalan, host siap membuat dan menjalankan virtual machine.

## 6. Memeriksa Status Libvirt

```bash
sudo systemctl status libvirtd.service
```

Memverifikasi apakah libvirtd berjalan dengan normal. </br>
Output menunjukkan status active (running), PID proses, dan log service.

## 7. Menambahkan User ke Grup Libvirt

```bash
sudo usermod -a -G libvirt $(whoami)
```

Memberikan hak akses kepada user agar dapat mengelola VM tanpa login sebagai root. </br>
Opsi -a -G menambahkan user ke grup libvirt tanpa menghapus grup lain yang sudah dimiliki.

## 8. Memuat Ulang Keanggotaan Grup

```bash
newgrp libvirt
```

Menerapkan keanggotaan grup libvirt tanpa logout/login. </br>
Shell baru akan menggunakan grup libvirt sebagai grup aktif.

## 9. Merestart Layanan Libvirt

```bash
sudo systemctl restart libvirtd.service
```

Menerapkan perubahan konfigurasi libvirt. </br>
Restart diperlukan setelah perubahan pada libvirtd.conf atau pengaturan jaringan.

## 10. Mengaktifkan Nested Virtualization (AMD)

```bash
sudo modprobe -r kvm_amd
sudo modprobe kvm_amd nested=1
```

Mengaktifkan fitur nested virtualization pada prosesor AMD. </br>
Fitur ini memungkinkan VM menjalankan hypervisor lain di dalamnya (VM di dalam VM).

## 11. Menyalin Image Sistem Operasi ke Direktori Libvirt

```bash
sudo cp <nama-file-image>.qcow2 /var/lib/libvirt/images/
```

Menyalin file image virtual machine ke direktori penyimpanan default milik **libvirt**. </br>
Perintah `cp` digunakan untuk menyalin file dari lokasi asal ke lokasi tujuan. Direktori `/var/lib/libvirt/images/` merupakan lokasi default yang digunakan libvirt untuk menyimpan disk image virtual machine. Dengan menempatkan image pada direktori tersebut, libvirt dapat mengakses dan menggunakannya saat membuat virtual machine baru.

## 12. Memverifikasi File Image Virtual Machine

```bash
ls -lh /var/lib/libvirt/images/
```

Menampilkan daftar file image virtual machine beserta ukuran dan hak aksesnya. </br>
Perintah `ls` digunakan untuk menampilkan isi direktori. </br>
- `-l` menampilkan informasi secara lengkap.
- `-h` menampilkan ukuran file dalam format yang mudah dibaca (KB, MB, GB).

Perintah ini digunakan untuk memastikan bahwa file image berhasil disalin ke direktori libvirt.

## 13. Menampilkan Informasi Interface Jaringan

```bash
ip a
```

Menampilkan seluruh interface jaringan beserta alamat IP yang dimiliki sistem. </br>
Perintah `ip a` (ip address) merupakan utilitas dari paket **iproute2** yang digunakan untuk melihat:
- Interface jaringan yang tersedia.
- Status interface (UP atau DOWN).
- Alamat IPv4 dan IPv6.
- MAC Address.

Informasi ini diperlukan sebelum melakukan konfigurasi firewall maupun jaringan virtual.

## 14. Menghentikan Layanan Firewalld

```bash
sudo systemctl stop firewalld
```

Menghentikan layanan **firewalld** yang sedang berjalan. </br>
Perintah ini menghentikan daemon firewalld sehingga aturan firewall tidak lagi diterapkan sampai layanan dijalankan kembali. </br>
Biasanya dilakukan sebelum mengubah konfigurasi firewall atau melakukan troubleshooting. </br>
Berkaitan dengan CIS Bab 4 mengenai konfigurasi firewall. Penghentian firewall sebaiknya hanya dilakukan sementara selama proses administrasi. </br>

## 15. Menjalankan Kembali Firewalld

```bash
sudo systemctl start firewalld
```

Menjalankan layanan **firewalld**. </br>
Perintah ini mengaktifkan kembali firewall sehingga seluruh aturan keamanan jaringan kembali diterapkan. </br>
Sesuai rekomendasi CIS agar sistem menggunakan host-based firewall untuk melindungi layanan yang berjalan.

## 16. Memeriksa Status Firewalld

```bash
sudo systemctl status firewalld
```

Memastikan layanan firewalld berjalan dengan normal. </br>
Output akan menampilkan status layanan, PID proses, penggunaan memori, serta log terakhir dari firewalld. </br>
Status **active (running)** menunjukkan firewall telah aktif.

## 17. Menampilkan Seluruh Zona Firewall

```bash
sudo firewall-cmd --list-all-zones
```

Menampilkan seluruh zona (zone) beserta aturan firewall yang telah dikonfigurasi. </br>
Firewalld membagi aturan keamanan ke dalam beberapa zona seperti:

- public
- trusted
- internal
- home
- work
- dmz
- external
- libvirt

Informasi yang ditampilkan meliputi service yang diizinkan, port yang dibuka, interface yang digunakan, hingga aturan tambahan (rich rules). </br>
Berhubungan dengan CIS Bab 4 mengenai konfigurasi firewall dan pembatasan akses jaringan.

---

## 18. Membuka Port Kubernetes API Server

```bash
sudo firewall-cmd --permanent --add-port=6443/tcp
```

Membuka port **6443/TCP** secara permanen pada firewall agar Kubernetes API Server dapat menerima koneksi dari node lain dalam cluster. </br>
Port **6443/TCP** merupakan port default yang digunakan oleh Kubernetes API Server. Pada implementasi K3s, port ini diperlukan agar node worker maupun administrator dapat berkomunikasi dengan control plane.

Opsi yang digunakan:

- `--permanent` : Menyimpan aturan firewall secara permanen sehingga tetap berlaku setelah sistem di-reboot.
- `--add-port` : Menambahkan port tertentu ke dalam aturan firewall.

Berhubungan dengan **CIS Section 4 (Host-based Firewall)**, yaitu hanya membuka port yang memang diperlukan oleh layanan yang dijalankan sehingga meminimalkan permukaan serangan (attack surface).

## 19. Memuat Ulang Konfigurasi Firewall

```bash
sudo firewall-cmd --reload
```

Menerapkan seluruh perubahan konfigurasi firewall yang telah disimpan. </br>
Perintah ini memuat ulang konfigurasi firewalld tanpa menghentikan layanan sehingga aturan baru, seperti pembukaan port, langsung diterapkan. </br>
Merupakan langkah verifikasi setelah perubahan konfigurasi firewall. Sesuai praktik hardening agar setiap perubahan benar-benar diterapkan.

## 20. Memverifikasi Konfigurasi Firewall

```bash
sudo firewall-cmd --list-all-zones
```

Memastikan aturan firewall telah diterapkan dengan benar. </br>
Setelah melakukan perubahan konfigurasi, administrator perlu memverifikasi bahwa:

- Port yang dibutuhkan telah terbuka.
- Interface berada pada zona yang benar.
- Service yang diizinkan sesuai kebutuhan.

Tahap ini merupakan bagian dari proses validasi konfigurasi keamanan.

## 21. Menginstal NFTables

```bash
sudo pacman -S nftables
```

Menginstal **nftables** sebagai framework firewall modern pada Linux. </br>
NFTables merupakan penerus iptables yang menyediakan mekanisme filtering paket jaringan dengan performa lebih baik dan konfigurasi yang lebih sederhana. </br>
Pada sistem Linux modern, nftables menjadi backend yang digunakan oleh berbagai utilitas firewall, termasuk firewalld. </br>
Berhubungan dengan **CIS Section 4 (Host-based Firewall)** karena nftables merupakan salah satu mekanisme yang dapat digunakan untuk mengamankan lalu lintas jaringan.

## 22. Merestart Firewalld

```bash
sudo systemctl restart firewalld
```

Menjalankan ulang layanan firewalld agar seluruh konfigurasi aktif kembali. </br>
Restart service diperlukan apabila terdapat perubahan konfigurasi yang membutuhkan inisialisasi ulang layanan firewall. </br>
Setelah restart selesai, seluruh aturan yang tersimpan akan dimuat kembali.

## 23. Memeriksa Status Firewalld Setelah Konfigurasi

```bash
sudo systemctl status firewalld
```

Memastikan layanan firewalld tetap berjalan setelah dilakukan perubahan konfigurasi. </br>
Output perintah akan menunjukkan apakah service berada pada status **active (running)** beserta informasi proses yang sedang berjalan. </br>
Tahap ini penting untuk memastikan firewall tetap aktif setelah proses konfigurasi selesai.

## 24. Verifikasi Akhir Aturan Firewall

```bash
sudo firewall-cmd --list-all-zones
```

Melakukan pemeriksaan akhir terhadap seluruh aturan firewall. </br>
Perintah ini memastikan bahwa:

- Zona firewall telah sesuai.
- Port yang dibutuhkan telah terbuka.
- Tidak terdapat aturan yang tidak diinginkan.

Verifikasi akhir dilakukan sebelum host digunakan sebagai bagian dari cluster Kubernetes.

## 25. Mengedit Konfigurasi Libvirt

```bash
sudo vim /etc/libvirt/libvirtd.conf
```

Membuka file konfigurasi utama **libvirtd** untuk dilakukan perubahan konfigurasi. </br>
Perintah ini membuka file `libvirtd.conf` menggunakan editor **Vim**. File tersebut berisi berbagai pengaturan daemon libvirt, seperti autentikasi, izin akses, socket, logging, dan parameter layanan lainnya. </br>
Perubahan pada file ini umumnya dilakukan sebelum layanan `libvirtd` di-restart agar konfigurasi baru diterapkan.

## 26. Mengaktifkan Nested Virtualization untuk Prosesor AMD

```bash
sudo modprobe kvm_amd nested=1
```

Memuat modul kernel `kvm_amd` dengan fitur **nested virtualization** diaktifkan. </br>
`modprobe` digunakan untuk memuat modul kernel Linux beserta parameternya.

Parameter:
- `nested=1` mengaktifkan kemampuan menjalankan hypervisor di dalam virtual machine (nested virtualization).
Fitur ini diperlukan apabila virtual machine akan digunakan untuk menjalankan teknologi virtualisasi lainnya, misalnya saat membuat cluster virtual di dalam VM.

## 27. Menyimpan Konfigurasi Nested Virtualization

```bash
echo "options kvm-intel nested=1" | sudo tee /etc/modprobe.d/kvm-intel.conf
```

Menyimpan konfigurasi parameter modul kernel agar tetap aktif setelah sistem di-boot ulang. </br>
Perintah ini membuat file konfigurasi pada direktori `/etc/modprobe.d/`.

**Catatan:**

Berdasarkan isi file `.cast`, sistem menggunakan prosesor **AMD** (`kvm_amd`), sehingga konfigurasi yang lebih sesuai adalah:

```bash
echo "options kvm_amd nested=1" | sudo tee /etc/modprobe.d/kvm_amd.conf
```

Perintah pada log menggunakan `kvm-intel`, sehingga kemungkinan merupakan salah ketik atau sisa dokumentasi dari perangkat lain.

## 28. Memverifikasi Status Nested Virtualization

```bash
systool -m kvm_amd -v | grep nested
```

Memastikan apakah fitur nested virtualization telah aktif. </br>
Perintah ini menampilkan informasi modul kernel `kvm_amd`, kemudian memfilter output agar hanya menampilkan parameter `nested`. </br>
Apabila bernilai `Y` atau `1`, maka nested virtualization telah aktif.

## 29. Berpindah ke Direktori Downloads

### Command

```bash
cd Downloads/
```

Berpindah ke direktori **Downloads**. </br>
Direktori ini digunakan sebagai lokasi penyimpanan image sistem operasi yang akan digunakan untuk membuat virtual machine.

## 30. Menampilkan Isi Direktori Downloads

```bash
ls
```

Menampilkan daftar file pada direktori aktif. </br>
Perintah ini digunakan untuk memastikan file image Ubuntu tersedia sebelum dipindahkan ke direktori libvirt.

## 31. Beralih Menjadi Pengguna Root

```bash
sudo su
```

Masuk ke shell sebagai pengguna **root**. </br>
Perintah ini memberikan hak administratif penuh sehingga administrator dapat melakukan operasi yang memerlukan izin root, seperti menyalin image ke direktori sistem. </br>
Penggunaan shell root sebaiknya dibatasi hanya saat diperlukan. </br>
Sesuai prinsip **least privilege** pada CIS, akses root sebaiknya digunakan seminimal mungkin dan hanya untuk tugas administrasi yang memerlukannya.

## 32. Memverifikasi Modul KVM yang Sedang Aktif

```bash
lsmod | grep kvm
```

Menampilkan modul kernel KVM yang sedang dimuat (loaded) pada sistem. </br>
Perintah `lsmod` menampilkan seluruh modul kernel yang aktif, sedangkan `grep kvm` menyaring hasil agar hanya menampilkan modul yang berkaitan dengan KVM, misalnya:

- `kvm`
- `kvm_amd`
- `kvm_intel`

Perintah ini digunakan untuk memastikan bahwa modul virtualisasi telah berhasil dimuat oleh kernel.

## 33. Memuat Modul Kernel KVM AMD

```bash
sudo modprobe kvm_amd
```

Memuat modul kernel KVM untuk prosesor AMD. </br>
Apabila modul belum aktif, perintah ini akan memuat modul `kvm_amd` sehingga fitur hardware virtualization dapat digunakan oleh QEMU/KVM.

## 34. Memverifikasi Kembali Modul KVM

```bash
lsmod | grep kvm
```

Memastikan modul `kvm_amd` telah berhasil dimuat. </br>
Setelah menjalankan `modprobe`, administrator perlu memverifikasi bahwa modul benar-benar aktif sebelum membuat virtual machine.

## 35. Merestart Layanan Libvirt

```bash
sudo systemctl restart libvirtd
```

Memulai ulang layanan libvirt setelah perubahan konfigurasi. </br>
Restart diperlukan agar perubahan pada konfigurasi daemon maupun modul virtualisasi dapat diterapkan.

## 36. Menjalankan Virtual Network Default

```bash
sudo virsh net-start default
```

Menjalankan jaringan virtual **default** yang dikelola oleh libvirt. </br>
`virsh` merupakan utilitas command line untuk mengelola libvirt. </br>
Perintah ini mengaktifkan jaringan virtual bernama **default**, sehingga virtual machine dapat memperoleh alamat IP melalui DHCP dan berkomunikasi melalui NAT.

## 37. Menginstal Libguestfs Tools

```bash
sudo pacman -S libguestfs
```

Menginstal utilitas **libguestfs** untuk melakukan administrasi disk image virtual machine. </br>
Paket ini menyediakan berbagai tool, seperti:

- `virt-customize`
- `virt-inspector`
- `guestfish`
- `virt-edit`

Tool tersebut memungkinkan administrator mengelola image VM tanpa harus menjalankannya.

## 38. Mengubah Password Root pada Disk Image

```bash
sudo virt-customize -a focal.img --root-password password:rahasia
```

Mengubah password akun **root** pada image virtual machine sebelum image dijalankan. </br>
`virt-customize` memungkinkan administrator melakukan modifikasi terhadap disk image secara offline.

Pada command ini:
- `-a focal.img` menentukan image yang akan dimodifikasi.
- `--root-password` mengatur password akun root.

Metode ini memudahkan proses provisioning virtual machine sebelum digunakan.

## 39. Mengaktifkan Fitur Masquerading pada Firewalld

```bash
firewall-cmd --zone=public --add-masquerade --permanent
```

Mengaktifkan fitur **IP Masquerading** pada zona `public` secara permanen. </br>
IP Masquerading merupakan teknik **Network Address Translation (NAT)** yang memungkinkan perangkat pada jaringan internal mengakses jaringan lain menggunakan alamat IP milik host. </br>
Pada implementasi KVM/libvirt maupun Kubernetes, fitur ini sering digunakan agar virtual machine atau node dapat mengakses internet melalui host.

Parameter yang digunakan:
- `--zone=public` : Mengubah konfigurasi pada zona **public**.
- `--add-masquerade` : Mengaktifkan NAT (Masquerading).
- `--permanent` : Menyimpan konfigurasi agar tetap berlaku setelah reboot.

Setelah menjalankan perintah ini, konfigurasi biasanya perlu dimuat ulang menggunakan:

```bash
sudo firewall-cmd --reload
```

Berkaitan dengan **CIS Section 4 (Host-Based Firewall)**, khususnya pengelolaan aturan firewall yang hanya mengizinkan lalu lintas sesuai kebutuhan layanan.

## 40. Mengaktifkan IPv4 Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

> **Catatan:** Pada file `.cast` tertulis `udo sysctl...`, yang kemungkinan merupakan salah ketik saat perekaman. Command yang benar adalah `sudo sysctl ...`.

Mengaktifkan **IPv4 Packet Forwarding** sehingga host dapat meneruskan paket jaringan antar-interface.

Parameter kernel:

```text
net.ipv4.ip_forward
```

menentukan apakah kernel Linux diperbolehkan meneruskan paket IPv4.

Nilai:
- `0` → forwarding dinonaktifkan.
- `1` → forwarding diaktifkan.

Konfigurasi ini diperlukan pada host yang berfungsi sebagai:
- Router
- NAT Gateway
- Host KVM
- Node Kubernetes
- Server Virtualisasi

Perintah ini hanya berlaku hingga sistem di-reboot. Agar permanen, konfigurasi dapat ditambahkan ke file:

```text
/etc/sysctl.conf
```

atau

```text
/etc/sysctl.d/*.conf
```

kemudian diterapkan menggunakan:

```bash
sudo sysctl --system
```
