# Installasi Podman pada Cluster Server

 **Menghubungkan ke internet:**

```
Sudo su
```
Masukan password user (Force)

```
Nvim /etc/subuid
```
systemctl enable --global podman
```
```
**membuat  folder konfigurasi
```
mkdir -p config/containers
```
```
sudo pacman -S coreutils 
```
```
ls config
```
nvim config/containers/storage.conf
```
Insert
```
Kemudian ketik manual dengan mengikuti gambar dibawah ini

<img width="1600" height="1200" alt="4b17721a-bad9-4c18-9826-8d2c4fc8584b" src="https://github.com/user-attachments/assets/5c008257-7a34-4670-8f8e-d21a6577f12b" />

```
esc
```
:wq
```
```
sudo nvim /etc/containers/registries
```
**Mengunduh image podman dari internet
```
podman pull quay.io/podman/stable
```

```
nvim config/containers/storage.conf
```

```
sudo nvim /etc/modprobe.d/01-hard.conf
```
Ketik manual dan sesuaikan pada gambar dibawah ini

<img width="1600" height="1200" alt="WhatsApp Image 2026-06-19 at 14 50 53" src="https://github.com/user-attachments/assets/2eaaba71-c1a8-49c5-8565-63ad595423c4" />



```
mkinitcpio -P
```

```
sudo su
```

```
podman pull quay.io/podman/stable
```
```
Kemudian cek status Podman untuk melihat apakah sudah berjalan atau belum
```
systemctl status podman
```

```
```
**Mengaktifkan podman
```
systemctl enable --now podman.service
```
```
systemctl enable --now podman.socket
```

**kemudian di cek Kembali dengan command**
```
systemctl status podman
```
**Mengaktifkan enable podman**

```
systemctl enable --now podman.service
```
```
podman run --privileged -it quaye.io/podman/stable podman --version


