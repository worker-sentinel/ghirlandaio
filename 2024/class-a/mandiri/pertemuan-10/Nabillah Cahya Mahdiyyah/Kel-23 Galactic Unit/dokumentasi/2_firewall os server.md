
## 1. Mengaktifkan Firewalld

```
systemctl enable firewalld
systemctl status firewalld
```

Mengaktifkan Firewalld dan memeriksa status service.

---

## 2. Memeriksa Zone Drop

```
firewall-cmd --info-zone=drop
```

Menampilkan informasi konfigurasi zone `drop`.

---

## 3. Memeriksa Zone Block

```
firewall-cmd --info-zone=block
```

Menampilkan informasi konfigurasi zone `block`.

---

## 4. Memeriksa Zone Public

```
firewall-cmd --info-zone=public
```

Menampilkan konfigurasi zone `public`.

---

## 5. Menghapus Service DHCPv6 Client dari Zone Public

```
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload
```

Menghapus service `dhcpv6-client` dari zone `public` dan menerapkan perubahan.

---

## 6. Memeriksa Zone External

```
firewall-cmd --info-zone=external
```

Menampilkan konfigurasi zone `external`.

---

## 7. Menghapus Service SSH dari Zone External

```bash
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `external`.

---

## 8. Memeriksa Zone Internal

```
firewall-cmd --info-zone=internal
```

Menampilkan konfigurasi zone `internal`.

---

## 9. Menghapus Service pada Zone Internal

```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `internal`.

---

## 10. Memeriksa Zone DMZ

```
firewall-cmd --info-zone=dmz
```

Menampilkan konfigurasi zone `dmz`.

---

## 11. Menghapus Service SSH dari Zone DMZ

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `dmz`.

---

## 12. Memeriksa Zone Work

```
firewall-cmd --info-zone=work
```

Menampilkan konfigurasi zone `work`.

---

## 13. Menghapus Service pada Zone Work

```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `work`.

---

## 14. Memeriksa Zone Home

```bash
firewall-cmd --info-zone=home
```

Menampilkan konfigurasi zone `home`.

---

## 15. Menghapus Service pada Zone Home

```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `home`.

