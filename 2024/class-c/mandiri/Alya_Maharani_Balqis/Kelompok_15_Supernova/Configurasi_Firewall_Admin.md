# Set up Firewall Admin
## Login ke Server

```
ssh kel10@15.15.5.7
```

## Membuat Zona Firewall Admin

```
sudo firewall-cmd --permanent --new-zone=admin
```

## Menambah IP ke Admin
```
sudo firewall-cmd --permanent --zone=admin --add-source=15.15.5.2
```

## Mengizinkan akses SSH ke Admin

```
sudo firewall-cmd --permanent --zone=admin --add-service=ssh
```

## Membuka akses zone Port 

```
sudo firewall-cmd --permanent --zone=admin --add-port=3306/tcp
```
```
sudo firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
```
sudo firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
```
sudo firewall-cmd --permanent --zone=admin --add-port=443/tcp
```
