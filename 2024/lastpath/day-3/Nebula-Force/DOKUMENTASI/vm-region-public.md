# Virtual Machine
## Install Virtual Machine paket utama
Untuk menginstall paket program utama gunakan command
```
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

## Install program tambahan
untuk menginstal program tambahan gunakan command
```
sudo pacman -S libguestfs
```

## Install alat informasi
menginstal program untuk melihat informasi komponen fisik dengan menggunakan command
```
sudo pacman -S dmidecode
```

## Install Program edit teks
menginstal program untuk menulis teks dokumen dengan menggunakan command
```
sudo pacman -S vim
```

## Lalu program otomatis nyala saat computer hidup
Mengatur berjalannya program secara otomatis dengan menggunakan command
```
sudo systemctl enable libvirtd.service
```

## Menyalakan layanan virtualisasi
menyalakan program agar bisa langsung dipakai dengan menggunakan command
```
sudo systemctl start libvirtd.service
```

## Mengecek status layanan virtualisasi
mengecek apakah program sudah menyala dengan menggunakan command
```
systemctl status libvirtd.service
```

## Membuka file pengaturan virtualisasi
membuka file program untuk mengubah isinya dengan menggunakan command
```
sudo vim /etc/libvirt/libvirtd.conf
```

## Memberikan izin akses ke akun pengguna
memberi akses pada akun dengan menggunakan command
```
sudo usermod -a -G libvirt $(whoami)
```

## Mengaktifkan izin akses baru
Mengaktifkan izin akses baru akun secara langsung dengan menggunakan command
```
newgrp libvirt
```

## Memuat ulang layanan virtualisasi
merestart program agar bisa langsung berjalan dengan menggunakan command
```
sudo systemctl restart libvirtd.service
```

## Mematikan fungsi virtualisasi prosesor intel
mematikan fungsi virtualisasi prosesor intel dengan meggunakan command
```
sudo modprobe -r kvm_intel
```

## Menyalakan fitur virtualisasi bertingkat
menyalakan Kembali fungsi prosesor intel dengan menggunakan command
```
sudo modprobe kvm_intel nested=1
```

## Menyimpan pengaturan virtualisasi bertingkat secara permanen
menyimpan pengaturan virtual ganda agar permanen dengan menggunakan command
```
echo "options kvm-intel nested=1" | sudo tee /etc/modprobe.d/kvm-intel.conf
```

## Memeriksa status fitur virtualisasi bertingkat
memastikan pengaturan virtual ganda sudah aktif dengan menggunakan command
```
systool -m kvm_intel -v | grep nested
```

## keluar 
```
Exit
```
