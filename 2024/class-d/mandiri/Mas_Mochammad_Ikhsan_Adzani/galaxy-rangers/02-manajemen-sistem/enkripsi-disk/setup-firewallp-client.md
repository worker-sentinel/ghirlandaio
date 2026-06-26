# Zona wifi-client

Misalnya static AP memberikan:

```
10.20.0.0/24
```

buat zona:

```
firewall-cmd --permanent --new-zone=wifi
```
```
firewall-cmd --permanent --zone=wifi --add-source=10.20.0.0/24
```

# WiFi Client

Hanya public:

```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
