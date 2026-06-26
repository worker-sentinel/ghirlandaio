# SET UP FIREWALL

## 1. Firewall 
> Cek status firewall
```
systemctl status firewalld
```
> hAASIL
```
Active: Active (running)
```
>Berwarna hijau

## 2. Melihat semua konfigurasi zone
```
firewall-cmd --list-all-zone
```
## 3. HApus service DHCPv6 dan SSH pada zone work
  ```
  firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
  ```
- Cek kembali konfigurasi zone
    ```
    firewall-cmd --list-all-zone
    ```
- Jika masih muncul pada, services: dhcpv6-client ssh. HArus reload firewall
    ```
    firewall-cmd --reload
    ```
## 4. GAteway
Menghapus service pada zone public
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,http,https} --permanent
```
- Reload firewall
  ```
  firewall-cmd --reload
  ```
## 5. Menghapus service pada zone Internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
- Reload firewall
  ```
  firewall-cmd --reload
  ```

 - Cek lagii
   ```
   firewall-cmd --list-all-zone
   ```
## 6. Menghapus SSH pada zone external
   ```
   firewall-cmd --zone=external --remove-service=ssh --permanent
   ```
- Reload
     ```
     firewall-cmd --reload
     ```
## 7. Menghapus SSH pada zone DMZ
```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```
- Reload firewall
  ```
  firewall-cmd --reload
  ```
## 8. Menghapus service pada zone nm-shared
   ```
   firewall-cmd --zone=nm-shared --remove-service={dhcp,dns,ssh} --permanent
   ```
   - Reload
     ```
     firewall-cmd --reload
     ```
  ## 9. LAST
   Cek lgii
   ```
   firewall-cmd --list-all-zone
   ```
   Slenajutnya
   ```
   exit
   ```
