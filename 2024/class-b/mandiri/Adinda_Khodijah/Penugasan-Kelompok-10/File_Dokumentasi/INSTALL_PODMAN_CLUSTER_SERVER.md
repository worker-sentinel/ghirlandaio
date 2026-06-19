# Installasi Podman pada Cluster Server

 **Menghubungkan ke internet:**

```
sudo su
```
Masukan password user (Force)

```
nvim /etc/subuid
```
```
systemctl enable --global podman
```

**membuat  folder konfigurasi**
```
mkdir -p config/containers
```
```
sudo pacman -S coreutils 
```
```
ls config
```
```
nvim config/containers/storage.conf
```
```
Insert
```
**Kemudian ketik manual dengan mengikuti gambar dibawah ini**
<img width="1600" height="900" alt="PODMAN 1" src="https://github.com/user-attachments/assets/737c04e4-ae29-42b9-8024-adfee37d864a" />

```
esc
```
```
:wq
```
```
sudo nvim /etc/containers/registries
```
**Mengunduh image podman dari internet**
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
 <img width="1600" height="900" alt="PODMAN 2" src="https://github.com/user-attachments/assets/6574abe4-5ac7-4881-b250-693f4cb5e098" />

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
```
systemctl status podman
```

```
**Mengaktifkan podman
```
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
