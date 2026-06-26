### Installasi & Konfigurasi OS Server (Arch Linux Hardened)

--- 

### Melihat Partisi Disk
```
lsblk
```

### Masuk sebagai Root
```
sudo su
```
Masukkan password


### Instalasi Kernel linux-hardened
```
pacman -S linux-hardened linux-hardened-headers
Packages (2) linux-hardened-7.0.12.hardened1-1  linux-hardened-headers-7.0.12.hardened1-1

Total Download Size:   175.58 MiB
Total Installed Size:  296.84 MiB

:: Proceed with installation? [Y/n] Y
```

### Konfigurasi mkinitcpio Preset

Edit file preset mkinitcpio untuk kernel hardened:

```
nvim /etc/mkinitcpio.d/linux-hardened.preset

Isi sesuai:

# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```

### Build UKI

```
mkinitcpio -P
==> Building image from preset: /etc/mkinitcpio.d/linux-hardened.preset: 'default'
==> Using configuration file: '/etc/mkinitcpio.conf'
  -> -k /boot/vmlinuz-linux-hardened -c /etc/mkinitcpio.conf -U /boot/efi/Linux/arch-linux-hardened.efi
==> Starting build: '7.0.12-hardened1-1-hardened'
  -> Running build hook: [base]
  -> Running build hook: [systemd]
  -> Running build hook: [autodetect]
  -> Running build hook: [microcode]
  -> Running build hook: [modconf]
  -> Running build hook: [kms]
  -> Running build hook: [keyboard]
  -> Running build hook: [sd-vconsole]
  -> Running build hook: [sd-encrypt]
==> WARNING: Possibly missing firmware for module: 'qat_6xxx'
  -> Running build hook: [lvm2]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [fsck]
==> Generating module dependencies
==> Creating zstd-compressed initcpio image
  -> Early uncompressed CPIO image generation successful
==> Initcpio image generation successful
==> Creating unified kernel image: '/boot/efi/Linux/arch-linux-hardened.efi'
==> Unified kernel image generation successful
```


### Verifikasi File EFI

```
ls /boot/efi/Linux/arch-linux-hardened.efi
```

```
exit
```
