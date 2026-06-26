# Zona wifi-client
Misalnya static AP memberikan:

text
> 10.20.0.0/24


buat zona:

bash
```
firewall-cmd --permanent --new-zone=wifi
```
```
firewall-cmd --permanent --zone=wifi --add-source=10.20.0.0/24
```

## WiFi Client

Hanya public:

bash
```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
