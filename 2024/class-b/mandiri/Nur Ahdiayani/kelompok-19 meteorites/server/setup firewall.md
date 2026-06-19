## konfigurasi firewalld
````
firewall-cmd --info-zone=drop
firewall-cmd --info-zone=
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload
firewall-cmd --zone=public --remove-service=ssh --permanent
firewall-cmd --reload
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --reload
firewall-cmd --zone=public --remove-service=http --permanent
firewall-cmd --reload
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
firewall-cmd --zone=dmz --remove-service=ssh  --permanent
firewall-cmd --reload
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh}  --permanent
firewall-cmd --reload
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
firewall-cmd --zone=public --remove-interface=wlan0 --permanent
firewall-cmd --reload
firewall-cmd --zone=public --remove-port={2377/tcp,7946/tcp,4789/tcp,8000/tcp,5432/tcp,6379/tcp} --permanent
firewall-cmd --reload
firewall-cmd --zone=public --add-service=ssh --permanent
firewall-cmd --reload

cek semuanya sudah terkonfigurasi, lalu exit
````
