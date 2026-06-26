## Membuat Direktori Konfigurasi Site
```
mkdir -p /etc/nginx/site-available
mkdir -p /etc/nginx/site-enabled
```
## Mengedit Konfigurasi Utama Nginx
```
nvim /etc/nginx/nginx.conf
```
## Membuat Konfigurasi Virtual Host
### Atom

```
nvim /etc/nginx/sites-available/atom.conf
```

### SLiMS

```
nvim /etc/nginx/sites-available/slims.conf
```
## Melihat Isi Direktori Home
```
ls
```
### Output
```text
Desktop    config-firewalld.cast  docker-swarm
Downloads  disable-kernel.cast    docker-swarm.cast
firewall-admin.cast  firewalld-server.cast
firewalld5.cast nginx-server2.cast
nginx2-server.cast server.cast
setup-ip-server.cast setup.cast
swap-to-hardened.cast
```
## Mengaktifkan Konfigurasi Virtual Host
```
ln -sf /etc/nginx/site-available/slims.conf /etc/nginx/site-enabled
```
## Merestart Layanan Nginx
```
systemctl restart nginx
```
## Menguji Konfigurasi Nginx
```
nginx -t
```
### Output
```text
2026/06/26 01:02:00 [warn] 4723#4723: could not build optimal types_hash,
you should increase either types_hash_max_size: 1024 or
types_hash_bucket_size: 64; ignoring types_hash_bucket_size

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
