# Lesspodman
## Masuk root
```
sudo su
```
>Masukkan password
## Mengedit file
```
nvim /etc/subuid
```
>Insert
```
sagitarius:100000:65536
```
## Mengedit file
```
nvim /etc/subgid
```
>Insert
```
sagitarius:100000:65536
```
## Mengaktifkan podman
```
systemctl enable --global podman
```
## Keluar root
```
exit
```
## Membuat folder konfigurasi
```
mkdir -p .config/containers
```
## Melihat direktori
```
ls
```
## Melihat semua file
```
ls -la
```
## Melihat isi folder config
```
ls .config
```
## Membuat konfigurasi penyimpanan podman
```
nvim .config/containers/storage.conf
```
>Insert
```
[storage]
driver = "overlay"
[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```
## Melihat isi baterai
```
cat /sys/class/power_supply/BAT0/capacity
```
## Registri image Podman
```
sudo nvim /etc/containers/regitries.conf
```
>Insert
```
unqualified-search-registries = ["docker.io"]
```
## Mengecek status podman
```
systemctl status podman en
```
## Menjalankan Podman
```
 sudo systemctl start podman
```
## Mengecek status Podman
```
systemctl status podman
```

exit
