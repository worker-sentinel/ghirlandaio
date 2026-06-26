## Setup Firewall Akhir
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="(IP server 2)" port port="8080"protocol="tcp" accept'
```
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="8.8.8.0/24" port port="22" protocol="tcp" accept'
```
```
sudo firewall-cmd --reload
```
```
exit
```
