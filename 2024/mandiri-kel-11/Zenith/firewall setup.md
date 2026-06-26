

1. Masuk ke mode root.

```bash
sudo su
```

2. Masukkan password root.

3. Melihat seluruh konfigurasi firewall yang aktif.

```bash
firewall-cmd --list-all-zones
```

4. Menghapus layanan DHCP Client pada zone **work** secara permanen.

```bash
firewall-cmd --zone=work --remove-service=dhcp-client --permanent
```

5. Menerapkan perubahan konfigurasi firewall.

```bash
firewall-cmd --reload
```

6. Memeriksa kembali konfigurasi firewall.

```bash
firewall-cmd --list-all-zones
```

7. Menghapus layanan DHCPv6 Client pada zone **work** secara permanen.

```bash
firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
```

8. Menerapkan perubahan konfigurasi firewall.

```bash
firewall-cmd --reload
```

9. Memeriksa kembali konfigurasi firewall.

```bash
firewall-cmd --list-all-zones
```

10. Memastikan layanan DHCPv6 Client telah terhapus dari zone **work**.

```bash
firewall-cmd --list-all-zones
```

11. Menerapkan ulang konfigurasi apabila diperlukan.

```bash
firewall-cmd --reload
```

12. Memeriksa kembali konfigurasi firewall.

```bash
firewall-cmd --list-all-zones
```

13. Menghapus layanan Cockpit pada zone **public** secara permanen.

```bash
firewall-cmd --zone=public --remove-service=cockpit --permanent
```

14. Menerapkan perubahan konfigurasi firewall.

```bash
firewall-cmd --reload
```

15. Keluar dari mode root.

```bash
exit
```
