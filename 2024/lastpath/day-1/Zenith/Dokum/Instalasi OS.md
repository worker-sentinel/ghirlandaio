### Memasangkan wi-fi

```iwctl```
```station wlan0 get-networks```
```station wlan0 connect (Nama Wi-Fi)```
```kemudian masukkan password wi-fi```
```exit```

### Melihat partisi Disk

```lsblk```

### Membuat enkripsi Luks
```cryptsetup luksFormat /dev/nvme0n1p6```

### Membuka enkripsi Luks
```cryptsetup luksOpen /dev/nvme0m1p6 meitantei```

### Membuat Physical Volume
```pvcreate /dev/mapper/Itsukinnieee```

### Membuat Volume Group
```vgcreate nobuchan /dev/mapper/Itsukinnieee```

### Membuat Logical Volume
```lvcreate -L 10G nobuchan -n root```
```lvcreate -L 5G nobuchan -n vars```
```lvcreate -L 1G nobuchan -n vlog```
```lvcreate -L 1G nobuchan -n vtmp```
```lvcreate -L 1G nobuchan -n vaud```
```lvcreate -L 7.1G nobuchan -n home```
```lvcreate -L 3.5G nobuchan -n podman```

### Memverifikasi Hasil
```lsblk```

### Membuat File System
```EFI Partition```
```mkfs.vfat -F32 -n BOOT /dev/nvme0n1p6 Root```
```mkfs.ext4 /dev//root```
```vars```
```vtmp```
```vaud```
```vlog```
```home```
```podman```

 ### Melakukan Mount Partisi
 ```Boot```
 ```mount --mkdir \ -o uid=0,gid=0,fmask=0077,dmask=0077 \
/dev/nvme0n1p6 /mnt/boot```
```Root```
  ```mount /dev/latte/root /mnt```
	```mount --mkdir -o rw,nodev,nosuid,relatime  /dev/nobuchan/vars /mnt/var```
	```mount --mkdir -o rw,nodev,nosuid,noexec,relatime  /dev/nobuchanvtmp /mnt/var/tmp```
	```mount --mkdir -o rw,nodev,nosuid,noexec,relatime  /dev/nobuchan/vlog /mnt/var/log```
	```mount --mkdir -o rw,nosuid,noexec,relatime  /dev/nobuchan/vaud /mnt/var/log/audit```
 ```mount --mkdir -o rw,nosuid,noexec,relatime  /dev/nobuchan/home /mnt/home```
	```mount --mkdir -o rw,nosuid,noexec,relatime  /dev/nobuchan/podman /mnt/podman```
