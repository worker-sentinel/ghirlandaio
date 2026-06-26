## sertup firewalld 

sudo su 

## start the firewalld 

systemctl enable --now firewalld

## check status 

systemctl status firewalld 

## check zone 

firewall-cmd --list-all-zone 

## allow port

firewall-cmd --zone=public --add-port=22/tcp --permanent 

## allow service

firewall-cmd --zone=public --add-service=http --permanent 

firewall-cmd --zone=public --add-service=https --permanent

firewall-cmd --reload 
