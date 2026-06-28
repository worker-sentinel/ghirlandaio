## cek list ip
```
ip ap
```

## cek config jaringan
```
nvim /var/lib/iwd/ikanmati.psk
```

value
```
[Security]                                                                                                                                                               
PreSharedKey=07409fc91b7d66d914529af1eb1d74c9d2538cfa561d3593d61e4f0cc5e04680                                                                                            
Passphrase=88888888                                                                                                                                           
SAE-PT-Group19=ff700b20ff1dce97b0df773104aee24b7bef4aa14719daaf0d2a6d0d913ceacc76efbd93ad003216f481d8a3e8a711d7f105bac6bdce5289315c8ee23656c50f                          
SAE-PT-Group20=50829b3d9e6af5939189411ddfb739b6d0e99f293c0af7a0a4df07043f95819b230f5eeab008c50c33211bbfb299edba4d31db0a93ad37977e46664d0c7d2cf0458d87ab3df678d46d125f41d7
9da5695ed0fcfd9a4f1d31b847c46ee66f3939                                                                                                                                   
                                                                                                                                                                         
                                                                                                                                                                         
[IPv4]                                                                                                                                                                   
Address=10.10.1.253                                                                                                                                                      
Netmask=255.255.255.0                                                                                                                                                    
Gateway=10.10.1.1   
```

## pindah  direktori iwd
```
cd /var/lib/iwd
```

## cek list
```
ls
```
value
```
'PAK AMBATUKAM.psk'   hotspot   ikanmati.psk
```

## config hosts
```
nvim /etc/hosts   
```
value 
```
# Static table lookup for hostnames.                                                                                                                                     
# See hosts(5) for details.                                                                                                                                              
127.0.0.1        localhost                                                                                                                                               
::1              localhost                                                                                                                                               
                                                                                                                                                                         
10.10.1.20  local.test  
```

