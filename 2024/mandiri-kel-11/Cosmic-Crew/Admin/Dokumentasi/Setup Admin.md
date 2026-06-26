## Setup Admin

Edit file `/etc/hosts`:

```
nvim /etc/hosts
```

Isi file:

```
# Static table lookup for hostnames.
# See hosts(5) for details.

127.0.0.1    localhost
::1          localhost

192.168.2.11 slims.test
```

Simpan perubahan, lalu keluar dari user admin:

```
exit
```
> [!NOTE]
> "192.168.2.11" diisi ip jaringan server 2 yang sudah dibuat sebelumnya
