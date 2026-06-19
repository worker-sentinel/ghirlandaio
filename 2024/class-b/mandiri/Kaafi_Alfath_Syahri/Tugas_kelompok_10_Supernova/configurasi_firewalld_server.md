# Konfigurasi Firewall Server

## Akses ke Server melalui SSH

Gunakan perintah berikut untuk masuk ke server:

```bash
ssh [username_admin]@[ip_server]
```

Masukkan password administrator ketika diminta.

## Menampilkan Seluruh Zone Firewall

Untuk melihat daftar seluruh zone yang tersedia pada Firewalld:

```bash
sudo firewall-cmd --list-all-zones
```

## Menghapus Service pada Zone Work

Hapus service `dhcpv6-client` dan `ssh` dari zone:

```bash
sudo firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```

Muat ulang konfigurasi firewall:

```bash
sudo firewall-cmd --reload
```

## Menghapus Service pada Zone Public

Hapus service `dhcpv6-client` dari zone:

```bash
sudo firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```

Muat ulang konfigurasi firewall:

```bash
sudo firewall-cmd --reload
```

## Menghapus Service pada Zone Home

Hapus service `dhcpv6-client`, `mdns`, `samba-client`, dan `ssh` dari zone:

```bash
sudo firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

Muat ulang konfigurasi firewall:

```bash
sudo firewall-cmd --reload
```

## Menghapus Service pada Zone Internal

Hapus service `dhcpv6-client`, `mdns`, `samba-client`, dan `ssh` dari zone:

```bash
sudo firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

Muat ulang konfigurasi firewall:

```bash
sudo firewall-cmd --reload
```

## Menghapus Service pada Zone External

Hapus service `ssh` dari zone:

```bash
sudo firewall-cmd --zone=external --remove-service=ssh --permanent
```

Muat ulang konfigurasi firewall:

```bash
sudo firewall-cmd --reload
```


