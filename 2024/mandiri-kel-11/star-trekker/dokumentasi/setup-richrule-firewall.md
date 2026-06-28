**di server 1**
```
systemctl enable --now firewalld
```
```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="ipadminyangnyambungkeaccesspointserver1" port="22" protocol="tcp" accept' --permanent
```
```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="ipadminyangnyambungkeaccesspointserver1" port="8081" protocol="tcp" accept' --permanent
```
```
firewall-cmd --reload
```

**di server 2**
```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="ipadminyangnyambungkeaccesspointserver" port="22" protocol="tcp" accept' --permanent
```

```
firewall-cmd --zone=public --add-port="80"/tcp
```
```
firewall-cmd --reload
```



