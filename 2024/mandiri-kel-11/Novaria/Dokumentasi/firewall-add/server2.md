```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="173.28.37.8" port port="22" protocol="tcp" accept' --permanent
```
```
firewall-cmd --zone=public --add-port="80"/tcp --permannet
```

```
firewall-cmd --reload
```

```
firewall-cmd --info-zone=public
```
