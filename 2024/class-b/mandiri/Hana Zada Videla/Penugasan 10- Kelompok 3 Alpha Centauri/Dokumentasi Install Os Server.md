# Instalasii Os Server
1. Melihat / Cek Partisi

   lsblk
   ```
2. Enskripsi Partisi (LUKS)
   
   cryptsetup luksFormat /dev/nvme0n1p6
# Ketik YES (kapital) untuk konfirmasi, lalu masukkan passphrase (Password)

cryptsetup luksOpen /dev/nvme0n1p6 proc
# Buka partisi terenkripsi dengan nama mapper "proc"
```
3. Setup LVM
```

pvcreate /dev/mapper/proc          
vgcreate saw /dev/mapper/proc      

# Buat Logical Volumes:
lvcreate -L 8G    saw -n root      
lvcreate -L 5G    saw -n vars      
lvcreate -L 1G    saw -n vlog
lvcreate -L 1G    saw -n vaud      
lvcreate -L 1.5G  saw -n vtmp      
lvcreate -l50%FREE saw -n home     
lvcreate -l100%FREE saw -n podman
```

4. Format
```

mkfs.ext4 /dev/saw/root

mkfs.vfat -F32 /dev/nvme0n1p5

mkfs.ext4 /dev/saw/vars

mkfs.ext4 /dev/saw/vlog

mkfs.ext4 /dev/saw/vaud

mkfs.ext4 /dev/saw/vtmp

mkfs.ext4 /dev/saw/home

mkfs.ext4 /dev/saw/podman
```

5. Mounting Semua Partisi
```

mount /dev/saw/root /mnt

mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p5 /mnt/boot 

mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/vars  /mnt/var

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vlog  /mnt/var/log

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vaud  /mnt/var/log/audit

mount --m -o rw,nodev,nosuid,noexec,relatime /dev/saw/vtmp  /mnt/var/tmp

mount --mkdir -o rw,nodev,relatime /dev/saw/home  /mnt/home

mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/podman /mnt/var/lib/containers
```

6. Install Package (Pacstrap)
```

pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh \ sudo pacman wget curl grep iwd podman
```

7. Konfigurasi Chroot
```

arch-chroot /mnt
```

8. Install Bootloader 
```

bootctl --path=/boot install  
mkinitcpio -P       
```

9. Enable Services
```

systemctl enable systemd-networkd

systemctl enable systemd-resolved

systemctl enable iwd 

systemctl enable firewalld

systemctl enable sshd
```

10. Selesai, Exit Chroot
```


exit  
# keluar dari chroot
```
