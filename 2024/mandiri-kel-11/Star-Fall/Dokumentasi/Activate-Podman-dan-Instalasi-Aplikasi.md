# PODMAN SERVER 2 DARI ADMIN

## SSH SERVER 1 DI ADMIN

lalu ssh di admin

```bash

Sudo systemctl status sshd

```
Jika sudah aktif maka OK, terus lanjut

Kalau belum aktif: 

```bash
Sudo systemctl enable sshd
```

Terus aktifkan:
```bash
ssh namaserver1@ipa
```

Lalu masukkan password

Cek kernel yang lagi digunakan, kita pakai hardened
```bash
uname -a
```

Mengaktifkan podman
```bash
Sudo systemctl enable --global podman
```

Apabila linux yang dipakai belum hardeened, maka install dulu hardenednya 
```bash
pacman -S
```

```bash
cd /etc/mkinitcpio.d
```

```bash
ls -la
```
Kalau ada si linux-hardened makan linux-hardened sudah terinstall, Dua kernel itu tidak apa-apa, nanti bisa dihapus lts nya.

Edit file
```bash
sudo nvim linux-hardened.preset
```

Ubah sesuai foto, tapi sesuaikan juga dengan file lts yang sebelumnya

img

Jika sudah, keluar dari mode insert dengan `esc` dan `:wq`

Update initramfs
Bash```
sudo mkinitcpio -P
```

Hapus file linux-lts-preset
Bash```
rm -fr linux-lts-preset
```

Lalu reboot
bash```
Sudo reboot
```

Setelah reboot ssh lagi dari laptop admin.

Verifikasi lagi
Bash```
uname -a
```

Aktifkan podman
```bash
Sudo systemctl enable --global podman
```

Masukkan docker.io ke dalam file podman
Bash```
sudo nvim /etc/containers/registries.conf
```

Pergi ke line 21, masuk ke mode insert dan hapus simbol tagar (#), diganti jadi `[“docker.io”]`
Keluar dari mode insert, `esc` dan simpan `:wq`

Pergi ke file, akses username dari kernell hardened
```bash
udo nvim /etc/sysctl.d/custom.conf
```

Masukkan akses username space di Linux-hardened, masuk mode insert
```bash
kernel.unprivileged_userns_clone=1
```

Keluar dari mode insert `esc` dan simpan `:wq`

Instalasi aplikasi 
```bash
sudo pacman -S podman-compose
```

Install docker compose untuk slims

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

Kalau ada masalah dengan wget, maka install wget dulu 
```bash
Pacman -S wget
```

Lalu coba install ulang docker compose untuk slims.

Setelah itu unzip file, kalau belum ada install unzip dulu `pacman -S unzip`

```bash
unzip master.zip
```

Cek hasil unzip
```bash
Ls
```

Dari unzip akan tercipta folder `docker-compose-for-slims-master` (folder ini penting).


Masuk ke directory docker compose
```bash
cd docker-compose-for-slims-master
```

cek apakah sudah masuk ke compose
```bash
ls
```

Ganti izin app
```bash
sudo chmod -R 777 app/slims
```

Jalankan isi dari file compose
```bash
sudo podman compose up -d
```

Jika error karena jam maka jalankan command 
```bash
```


Mengecek apakah containers slims sudah berjalan
```bash
sudo podman ps -a 
```

Hentikan firewalld
```bash
Sudo systemctl stop firewalld
```

Buka ip server
```bash
ip a 
```

Buka browser lalu ketik `http://ipserver:80` untuk memunculkan tampilam installer slims. Apabila sudah ada, maka containers sudah running 

img
