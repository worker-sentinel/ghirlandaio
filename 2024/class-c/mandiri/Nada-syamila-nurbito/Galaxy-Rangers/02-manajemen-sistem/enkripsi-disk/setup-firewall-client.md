# Zona wifi-client

Misalnya static AP memberikan:

text
> 10.20.0.0/24


buat zona:

```
firewall-cmd --permanent --new-zone=wifi
```
```
firewall-cmd --permanent --zone=wifi --add-source=10.20.0.0/24
```
# Hak Akses tiap zona
## WiFi Client

Hanya public:
```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
