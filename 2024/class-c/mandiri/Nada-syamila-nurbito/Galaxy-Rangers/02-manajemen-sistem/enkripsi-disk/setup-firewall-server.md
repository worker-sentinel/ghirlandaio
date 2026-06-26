# firewall server
# buka ssh server
```
ssh [username admin] @[ip server]
```

## list semua zone
```
sudo firewall-cmd --list-all-zone
```

### hapus service zone work
```
sudo firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```

```
sudo firewall-cmd --reload
```
## hapus service zone public
```
sudo firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```

```
sudo firewall-cmd --reload
```

### hapus service zone home
```
sudo firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
```
sudo firewall-cmd --reload
```
### hapus service zone internal
```
sudo firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

```
sudo firewall-cmd --reload
```

### hapus service zone external
```
sudo firewall-cmd --zone=external --remove-service=ssh --permanent
```

```
sudo firewall-cmd --reload
```

