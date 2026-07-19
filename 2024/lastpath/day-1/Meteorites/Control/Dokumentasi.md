## 1.Cek partisi menggunakan:
```
lsblk
```
**Penjelasan:** Menampilkan partisi yang tersedia di dalam sistem (device masing - masing). ini di lakukan pada saat tahapan awal agar target partisi yang akan dipakai sudah benar sebelum melakukan operasi serta menghindari peghapusan diks lain.
---
## 2. Membuat physical volume menggunakan:
```
pvcreate /dev/[nama partisi]
```
**Penjelasan:** Membuat physical volume yang akan menampung logical volume yang ada ddi volume grup
---
## 3. Membuat volume group:
```
vgcreate [nama grup] /dev/[nama partisi]

```
**Penjelasan:** Membuat volume grup yang akan berisikan logical volume
---
## 4. Membuat logical volume
```
lvcreate -L [size G | M] [nama grup] -n root
lvcreate -L [size G | M] [nama grup] -n vars
lvcreate -L [size G | M] [nama grup] -n vlog
lvcreate -L [size G | M] [nama grup] -n vaud
lvcreate -L [size G | M] [nama grup] -n vtmp
lvcreate -L [size G | M] [nama grup] -n home
lvcreate -L [size G | M] [nama grup] -n podman
lvcraete -l50%FREE [nama grup] -n [nama home untuk internal]
```
**Penjelasan:** 
-`-L [size G | M]` : menentukan ukuran tetap setiap dari logical volume agar setiap partisi mempunyai kapasitasnya masing - masing
-`-n` [nama] : dipisah setiap partisi agar direktori yang penuh tidak emamtikan seluruh sistem dan diberi keamanan yang berbeda sesuai fungsinya
-`home internal` : tempat penyimpanan data khusus untuk pengguna internal pada server
---
## 5. Membuat Format Partisi:
```
mkfs.ext4 /dev/[nama grup]/root
mkfs.ext4 /dev/[nama grup]/vars
mkfs.ext4 /dev/[nama grup]/vlog
mkfs.ext4 /dev/[nama grup]/vaud
mkfs.ext4 /dev/[nama grup]/vtmp
mkfs.ext4 /dev/[nama grup]/home
mkfs.ext4 /dev/[nama grup]/podman
```
**Penjelasan:** Menformat partisi boot kita setelah kita menformat partisi
---
## 6. Format Partisi vfat
```
mkfs.vfat -F32 /dev/[nama grup]
```
**Pejelasan:** menformat partisi boot kita setelah kita menformat partisi
---
## 7. Membuat mounting
```
mount /dev/internal/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi_boot) /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/internal/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/internal/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/internal/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/internal/podman /mnt/var/lib/containers
```
**Penjelasan:** untuk mounting partisi root 
---
## 7. Membuat Luks Format
```
cruptsetup luksFormat /dev/[nama grup]/[nama home internal]
```
**Penjelasan;** Menformat nama grup yang menjadi partisi kita menjadi home internal
--
## 8. install package
```
