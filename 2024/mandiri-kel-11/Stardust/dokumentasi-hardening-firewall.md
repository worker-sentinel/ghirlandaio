
## Set up firewalld

```bash
firewall-cmd --list-all-zones
firewall-cmd --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```
## Reload firewall
firewall-cmd --reload

## Untuk check terhadap zona yang diinginkan
firewall-cmd --zone=(nama zona) --list-all
