```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.32.43.10" port port="22" protocol="tcp" accept' --permanent
```

```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="173.33.46.9" port port="0001" protocol="tcp" accept' --permanent
```
```
firewall-cmd --reload
```

```
firewall-cmd --info-zone=public
```
