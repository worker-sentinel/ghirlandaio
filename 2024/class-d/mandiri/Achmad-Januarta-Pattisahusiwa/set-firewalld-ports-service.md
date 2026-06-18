systemctl enable --now firewalld 

systemctl status firewalld 

firewalld-cmd --zome=public --add-service=http --permanent 

firewalld-cmd --zome=public --add-port=2377/tcp --permanent 

firewalld-cmd --zome=public --add-port=2377/tcp --permanent 

firewalld-cmd --zome=public --add-port=7946/tcp --permanent

firewalld-cmd --zome=public --add-port=4789/tcp --permanent 

firewalld-cmd --zome=public --add-port=8000/tcp --permanent 

firewalld-cmd --zome=public --add-port=5432/tcp --permanent 

firewalld-cmd --zome=public --add-port=6379/tcp --permanent 

firewalld-cmd --reload

firewalld-cmd --list-ports 
