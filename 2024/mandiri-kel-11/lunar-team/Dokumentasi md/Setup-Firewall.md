# Remove Firewall Server

## 1. Cek Status Firewall

```bash
sudo systemctl status firewalld
```

## 2.Cek Zona Firewall
```bash
sudo firewall-cmd --list-all-zone                                                                                                                        
```

## 3. Hapus Service di setiap zona firewall
```bash
sudo firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
sudo firewall-cmd --reload                                                                                                                               
sudo firewall-cmd --info-zone=work

sudo firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --info-zone=public

sudo firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
sudo firewall-cmd --reload  
sudo firewall-cmd --info-zone=internal

sudo firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
sudo firewall-cmd --reload  
sudo firewall-cmd --info-zone=home

sudo firewall-cmd --zone=external --remove-service=ssh --permanent
sudo firewall-cmd --reload  
sudo firewall-cmd --info-zone=external

sudo firewall-cmd --zone=dmz --remove-service=ssh --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --info-zone=dmz
```

## 4. Cek Apakah zonanya sudah bersih atau belum
```bash
sudo firewall-cmd --list-all-zone
```

# Add Port Server 

## 1. Cek Status Firewall
```bash
systemctl status firewalld
```

## 2. Tambahkan port di zone public
```bash
firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.10.10.2" port port="8080" protocol="tcp" accept'
firewall-cmd --reload
firewall-cmd --info-zone=public 

firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="5.5.5.0/24" port port="22" protocol="tcp" accept'
firewall-cmd --reload
firewall-cmd --info-zone=public 
```

