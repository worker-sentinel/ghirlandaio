*Set up firewall server*

## Login ke Server

```
ssh kel10@15.15.5.7
```

## Membuat Zona Firewall Admin

sudo su 
```
 firewall-cmd --permanent --new-zone=admin
```

## Menambah IP ke Admin
```
 firewall-cmd --permanent --zone=admin --add-source=15.15.5.2
```

## Mengizinkan akses SSH ke Admin

```
 firewall-cmd --permanent --zone=admin --add-service=ssh
```

## Membuka akses zone Port 

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
### membuat zona firewalld operator 

```
 firewall-cmd --permanent --new-zone=operator 
```
### izinin ssh 
```
 firewall-cmd --permanent --zone=operator --add-service=ssh
```
```
 firewall-cmd --permanent --zone=operator --add-source=15.15.5.3
```
## port untuk operator 
```
 firewall-cmd --permanent --zone=operator --add-port=3306/tcp
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
## membuat zona wifi 
```
 firewall-cmd --permanent --new-zone=wifi
```
```
 firewall-cmd --permanent --zone=wifi --add-source=15.15.5.4/24
```
```
 firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```

firewall-cmd --reload 


