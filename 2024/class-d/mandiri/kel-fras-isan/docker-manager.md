# service
```
systemctl enable --now docker.service
```
```
systemctl start --now docker.service
```

```
docker swarm init --advertise-addr [ip address]
```
```
Swarm initialized: current node (menyesuaikan) is now a manager.                                                                                                                 
                                                                                                                                                                                              
To add a worker to this swarm, run the following command:                                                                                                                                     
                                                                                                                                                                                              
    docker swarm join --token [tokennya] [ip address]                                                      
                                                                                                                                                                                              
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.   
```
> akan muncul output seperti ini
