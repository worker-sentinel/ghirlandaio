```
sudo firewall-cmd  --zone=public --add-rich-rule=‘rule family=“IPv4” source address=“[IP server 2]” port port=“8080” protocol=“tcp” accept’
sudo firewall-cmd  --zone=public --add-rich-rule=‘rule family=“IPv4” source address=“[IP server 2]” port port=“22” protocol=“tcp” accept’
```
