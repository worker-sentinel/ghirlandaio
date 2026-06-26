# check interface for ethernet


nmcli device status


# create admin user connection


nmcli connection add type ethernet ifname [interface] con-name "admin-connection" ipv4.method manual ipv4.addresses [ip address for admin]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8



# create operator user connection


nmcli connection add type ethernet ifname [interface] con-name "operator-connection" ipv4.method manual ipv4.addresses [ip address for operator]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8


# automated login script


echo "nmcli connection up admin-connection" >> /home/admin/.bash_profile



# automated login script


echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
