## buat zone admin
```
sudo firewall-cmd --permanent --new-zone=admin
```
```
sudo firewall-cmd --reload
```
```
sudo firewall-cmd --permanent --zone=admin --add-source=10.10.1.3
```

## buat zona operator
```
sudo firewall-cmd --permanent --new-zone=operator
```
```
sudo firewall-cmd --permanent --zone=operator --add-source=10.10.1.4
```

## buat zona client
```
sudo firewall-cmd --permanent --new-zone=wifi
```
```
firewall-cmd --permanent --zone=wifi --add-source=11.11.1.3/24
```

## beri hak akses di setiap zona sesuai dengen scema tugas


#### untuk admin

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

#### untuk operator

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

#### untuk public
```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```

### set default zone drop
```
firewall-cmd --set-default-zone=drop --permanent
```
```
firewall-cmd --reload
```
