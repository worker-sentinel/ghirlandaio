### SET FIREWALL SERVER 1 
```
sudo firewall-cmd –zone=public –add-rich-rule='rule family=”ipv4” source address=”[ip server 2]” port port=”8080” protocol=”tcp” accept'
```
```
sudo firewall-cmd –zone=public –add-rich-rule='rule family=”ipv4” source address=”[ip wifi server 1]” port port=”22” protocol=”tcp” accept’
```
```
sudo firewall-cmd –-reload
```

