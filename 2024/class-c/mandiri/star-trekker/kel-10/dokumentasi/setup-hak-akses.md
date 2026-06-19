## buat zone admin
```
firewall-cmd --permanent --new-zone=admin
```
```
firewall-cmd --permanent --zone=admin --add-source=10.10.1.3
```

## buat zona operator
```
firewall-cmd --permanent --new-zone=operator
```
```
firewall-cmd --permanent --zone=operator --add-source=10.10.1.4
```

## buat zona client
```
firewall-cmd --permanent --new-zone=wifi
```
```
firewall-cmd --permanent --zone=wifi --add-source=11.11.1.3/24
```
