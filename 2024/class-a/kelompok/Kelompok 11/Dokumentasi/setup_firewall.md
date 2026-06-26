
## 1. Mengaktifkan Firewalld

```
systemctl enable firewalld
systemctl status firewalld
```

---

## 2. Memeriksa Zone Drop

```
firewall-cmd --info-zone=drop
```

---

## 3. Memeriksa Zone Block

```
firewall-cmd --info-zone=block
```

---

## 4. Memeriksa Zone Public

```
firewall-cmd --info-zone=public
```

---

## 5. Menghapus Service Client dari Zone Public

```
firewall-cmd --zone=public --remove-service=client --permanent
firewall-cmd --reload
```

---

## 6. Memeriksa Zone External

```
firewall-cmd --info-zone=external
```

---

## 7. Menghapus Service SSH dari Zone External

```bash
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --reload
```

---

## 8. Memeriksa Zone DMZ

```
firewall-cmd --info-zone=dmz
``` 

## 9. Menghapus Service SSH dari Zone DMZ

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload
```

## 10. Memeriksa Zone Work

```
firewall-cmd --info-zone=work
```
---

## 11. Menghapus Service pada Zone Work

```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload
```
---

## 12. Memeriksa Zone Home

```bash
firewall-cmd --info-zone=home
```
---

## 13. Menghapus Service pada Zone Home

```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```
## 14 Keluar dari sesi
```
exit
```


