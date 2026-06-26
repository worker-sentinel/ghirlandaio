# Zona operator

```
firewall-cmd --permanent --new-zone=operator
```
```
firewall-cmd --permanent --zone=operator --add-source=10.10.1.3
```

## Operator
```
firewall-cmd --permanent --zone=operator --add-service=ssh
```
```
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```
```
firewall-cmd --permanent --zone=operator --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=operator --add-port=443/tcp
``` 
