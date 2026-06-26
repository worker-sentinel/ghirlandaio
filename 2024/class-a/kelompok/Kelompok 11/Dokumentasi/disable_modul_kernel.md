## 1. Mengedit File Konfigurasi Modul

```
nvim /etc/modprobe.d/hardening.conf
```

Mengedit file konfigurasi untuk menonaktifkan modul kernel yang tidak diperlukan.

```text
install cramfs /bin/false

install freevxfs /bin/false

install hfs /bin/false

install hfsplus /bin/false

install jffs2 /bin/false

install udf /bin/false

install usb-storage /bin/false

install firewire-core /bin/false
```

lalu keluar dengan 

```
klik esc
:wq
```

## 2. Membuat Ulang Initramfs

```
mkinitcpio -P
```

## 3. Cek Filesystem

```
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep firewire-core
```
---

## 4. Keluar dari Sesi

```
exit
```

