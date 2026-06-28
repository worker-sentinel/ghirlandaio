# PODMAN SERVER 2 DARI ADMIN

## SSH SERVER 1 LEWAT DEVICE ADMIN

```
sudo systemctl status sshd

```
> Jika sudah aktif maka OK, terus lanjut
> Kalau belum aktif: 
```
sudo systemctl enable sshd
```
> Terus aktifkan:
```
ssh namaserver1@ipa
```
> Cek ulang kernel yang lagi digunakan dan harus pakai lts-hardened
```
uname -a
```
> Mengaktifkan podman
```
sudo systemctl enable --global podman
```
> Apabila linux yang dipakai belum hardeened, maka harus diinstall kembali 
```
pacman -S
```
```
cd /etc/mkinitcpio.d
```
```
ls -la
```
> Mengkonfigurasi preset hardened
> Edit file
```
sudo nvim linux-hardened.preset
```

> Ubah sesuai foto, tapi sesuaikan juga dengan file lts yang sebelumnya

<img width="4096" height="3072" alt="image" src="https://github.com/user-attachments/assets/f887da85-318c-4977-aeec-d424b4992eab" />

> Jika sudah, keluar dari mode insert dengan `esc` dan `:wq`

> Update initramfs
```
sudo mkinitcpio -P
```
> Hapus file linux-lts-preset
```
rm -fr linux-lts-preset
```
> Lalu reboot
```
sudo reboot
```
> Setelah reboot ssh lagi dari laptop admin.
```
uname -a
```

> Aktifkan podman
```
sudo systemctl enable --global podman
```

> Masukkan docker.io ke dalam file podman
```
sudo nvim /etc/containers/registries.conf
```

> Pergi ke line 21, masuk ke mode insert dan hapus simbol tagar (#), diganti jadi `[“docker.io”]`
Keluar dari mode insert, `esc` dan simpan `:wq`

> Pergi ke file, akses username dari kernell hardened
```
sudo nvim /etc/sysctl.d/custom.conf
```

> Masukkan akses username space di Linux-hardened, masuk mode insert
```
kernel.unprivileged_userns_clone=1
```

> Keluar dari mode insert `esc` dan simpan `:wq`

> Instalasi aplikasi 
```
sudo pacman -S podman-compose
```

>Install docker compose untuk slims

```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

> Kalau ada masalah dengan wget, maka install wget dulu 
```
pacman -S wget
```

> Lalu coba install ulang docker compose untuk slims.
> Setelah itu unzip file, kalau belum ada install unzip dulu `pacman -S unzip`

```
unzip master.zip
```

> Cek hasil unzip
```bash
Ls
```

> Dari unzip akan terbuat folder `docker-compose-for-slims-master` 
> Masuk ke directory docker compose
```
cd docker-compose-for-slims-master
```

> cek apakah sudah masuk ke compose
```bash
ls
```

> Ganti izin app
```
sudo chmod -R 777 app/slims
```

> Jalankan isi dari file compose
```
sudo podman compose up -d
```

> Mengecek apakah containers slims sudah berjalan
```
sudo podman ps -a 
```

> Hentikan firewalld
```
sudo systemctl stop firewalld
```

> Buka ip server
```
ip a 
```

> Buka browser lalu ketik 
```
http://(ipserver):80`
```

<img width="1599" height="999" alt="image" src="https://github.com/user-attachments/assets/0bab2207-9c3d-4394-a16a-69fbc4a8a617" />

