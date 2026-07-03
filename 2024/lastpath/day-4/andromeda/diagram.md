
<img width="698" height="291" alt="Screenshot 2026-07-03 062147" src="https://github.com/user-attachments/assets/6ba897be-0b2e-46e1-a23c-23fa19848e13" />

admin, control, dan servem vm (kvm) adaalah satu pc yang sama yaitu zhafarina tijani athaya, region internal yaitu sayyida nafisa, region data yaitu pratama zumar, region public yaitu areva dzikra hody, client yaitu puput trimaililah, server vm lainnya yaitu allyssa ramadha.

region internal = cache (redis)

region data = mariadb

public = nginx

server vm = kvm, atom, qemu region

## Admin, Control, Server VM (KVM)
Admin: mengatur akses, user, dan konfigurasi keseluruhan sistem
Control: mengontrol/mengatur VM lain (start, stop, monitoring resource)
Server VM (KVM): hypervisor yang menjalankan virtual machine-virtual machine lainnya
Karena dia terhubung ke Region Data dan qemu data, artinya admin sekaligus server vm yang mendistribusikan/mengelola data ke seluruh region.

## Region Internal
region yang gak boleh diakses dari luar, fungsinya untuk menyimpan data sementara biar akses lebih cepat, gak perlu bolak-balik ke database utama. 

## Region Data
Tempat penyimpanan/pengolahan data utama (database) yang menjadi jembatan antara Region Internal (cache) dan Region Public.

## Region Public
Tempat yang ada web server atau API gateway yang bisa diakses client dari luar jaringan.

## Client
Ini pengguna akhir yang mengakses sistem lewat Region Public.

## qemu internal / qemu data / qemu public / client 
Versi virtual machine (vm) yang dijalankan pakai QEMU (emulator/virtualizer).

pada diagram topologi pada bagian atas adalah untuk k3s/kubernetes untuk slims. diagram topologi bagian bawah adalah kvm untuk atomn
