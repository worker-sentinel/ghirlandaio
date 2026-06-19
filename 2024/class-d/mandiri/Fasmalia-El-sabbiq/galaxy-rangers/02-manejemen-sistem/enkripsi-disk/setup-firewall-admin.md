# Zona admin
```
firewall-cmd --permanent --new-zone=admin
```
```
firewall-cmd --permanent --zone=admin --add-source=10.10.1.2
```
# Hak akses tiap zona

## Admin

```
firewall-cmd --permanent --zone=admin --add-service=ssh
```
```
firewall-cmd --permanent --zone=admin --add-port=3306/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=443/tcp
```
