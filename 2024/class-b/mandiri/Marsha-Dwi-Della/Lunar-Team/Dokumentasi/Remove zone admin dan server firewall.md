---

## 1. Akses Root

```bash
sudo su

```
## 2. remove zone
### A. Zona public (Zona Default)

```bash
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload

```
### B. Zona external & dmz
Menutup celah akses masuk jarak jauh melalui protokol ssh pada zona yang langsung berhadapan dengan jaringan publik luar.
```bash
# Menghapus SSH dari zona external
firewall-cmd --zone=external --remove-service=ssh --permanent

# Menghapus SSH dari zona dmz
firewall-cmd --zone=dmz --remove-service=ssh --permanent

# Memuat ulang konfigurasi firewall
firewall-cmd --reload

```
### C. Zona work
Menghapus sekaligus dua layanan, yaitu dhcpv6-client dan ssh, menggunakan ekspansi kurung kurawal {}
```bash
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload

```
### D. Zona home & internal 
Layanan yang dihapus meliputi: dhcpv6-client, mdns, samba-client, dan ssh.
```bash
# Pengerasan pada zona home
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent

# Pengerasan pada zona internal
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent

# Memuat ulang konfigurasi firewall
firewall-cmd --reload

```

```bash
firewall-cmd --list-all-zones

```
