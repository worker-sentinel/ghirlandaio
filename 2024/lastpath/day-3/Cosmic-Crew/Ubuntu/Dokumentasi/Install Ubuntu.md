# Menyiapkan Ubuntu Cloud Image untuk KVM/Libvirt

## Pindahkan Image ke Direktori Libvirt

Cek isi direktori home.

```
ls
```

Masuk ke folder Downloads.

```
cd Downloads/
```

Cek file yang tersedia di folder Downloads.

```
ls
```

Rename file image Ubuntu agar lebih singkat.

```
mv ubuntu-24.04-server-cloudimg-amd64.img ubuntu.img
```

Cek ulang hasil rename.

```
ls
```

Masuk ke direktori libvirt.

```
cd /var/lib/libvirt/
```

Pindahkan image ke direktori images milik libvirt.

```
mv ubuntu.img /var/lib/libvirt/images/
```

## Kustomisasi Image dengan guestfs-tools

Instal guestfs-tools untuk menjalankan virt-customize.

```
pacman -S guestfs-tools
```

Set password root pada image tanpa perlu boot VM.

```
virt-customize -a ubuntu.img --root-password password:250805
```

## Menjalankan libvirtd

Cek status layanan libvirtd.

```
systemctl status libvirtd
```

Jalankan libvirtd jika belum aktif.

```
systemctl start libvirtd
```

Cek ulang status libvirtd.

```
systemctl status libvirtd
```

## Memastikan Modul KVM Aktif

Muat modul kernel kvm.

```
sudo modprobe kvm
```

Cek apakah modul kvm sudah termuat.

```
lsmod | grep kvm
```

Restart libvirtd agar seluruh perubahan diterapkan.

```
sudo systemctl restart libvirtd
```

Keluar dari sesi shell.

```
exit
```
