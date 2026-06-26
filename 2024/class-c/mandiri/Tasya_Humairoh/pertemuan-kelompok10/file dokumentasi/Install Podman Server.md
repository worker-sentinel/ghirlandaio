## Installasi Podman pada Cluster Server

 **Menghubungkan ke internet:**

```
sudo su
```
**Masukan password user (Force)**

```
nvim /etc/subuid
```
```
systemctl enable --global podman
```
## Membuat  folder konfigurasi

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

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b0f9fc77-9453-4549-b965-1e88d57bf560" />

 
```
esc :wq
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
**Ketik manual dan sesuaikan pada gambar dibawah ini**
 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b9bfc145-ea53-4f85-a475-80df500dd31e" />


```
mkinitcpio -P
```

```
sudo su
```

```
podman pull quay.io/podman/stable
```

## Kemudian cek status Podman untuk melihat apakah sudah berjalan atau belum

```
systemctl status podman
```

**Mengaktifkan podman**

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
podman run --privileged -it quaye.io/podman/stable podman –version
```
