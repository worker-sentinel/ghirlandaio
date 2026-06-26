# INSTALISASI OS

## Connect Wifi
```
iwctl
```
```
device list
```
```
station wlan0 get-networks
```
```
station wlan0 connect oooal
```
```
exit
```

## Partisi Luks Root
```
cryptsetup luksOpen /dev/nvme0n1p8 cleo
```
mkfs.ext4 /dev/proc/root
```
mount /dev/proc/root /mnt
```
## Partisi Boot
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```

## Partisi Vars, tmp, vlog, vaud, dock, home
```
mkfs.ext4 /dev/proc/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```
```
mkfs.ext4 /dev/proc/vtmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec
