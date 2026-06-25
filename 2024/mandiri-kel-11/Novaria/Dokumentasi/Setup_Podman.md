# kita enable terlebih dahulu podman nya secara global
```
sudo systemctl enbale --global podman
```
# lalu kita masuk ke dalam directory containernya
```
cd /etc/containers
```
# lalu masuk ke configuration
```
sudo nvim /etc/containers/registries.conf
```
### lalu cari ada tulisa "unqualified"
<img width="357" height="369" alt="image" src="https://github.com/user-attachments/assets/6e491ef1-fec2-42fe-9f0e-364fc061448a" />
lalu bagian tersebut di hapus pagarnya dan "example.com" di ganti menajdi "docker.io"

## setelah itu buar file configuration
```
sudo nvim /etc/sysctl.d/[namafile.conf]
```
lalu isi
```
kernel.unprivileged_userns_clone=1
```
# lalu cek apakah file directorynya
```
sysctl --system
```
<img width="414" height="301" alt="image" src="https://github.com/user-attachments/assets/79114f63-f686-43f0-bc48-6aac7396511f" />

# install podman-compose nya
```
sudo pacman -S podman-compose
```








```
mkdir nova                          
cd nova/
```
```                                                                                                                                      
mkdir omeka                                                                                                                                            
cd omeka/    
```
```
mkdir config                                                                                                                                          
mkdir files                                                                                                                                           
ls                                                                                                                                                    
config  files  
```
```                                                                                                                                                      
chmod -R 777 config/                                                                                                                                  
chmod -R 777 files/
```
```                                                                                                                       
ls -la                                                                                                                                                                                                                                                                                    
cd config/                                                                                                                                            
ls                                                                                                                                                   
nvim database.ini
```

<img width="71" height="34" alt="Screenshot 2026-06-25 202346" src="https://github.com/user-attachments/assets/4ed1c874-067e-4fa0-9ffd-cd19c6117acc" />

```
cd ..                                                                                                                                                
nvim docker-compose.yml
```

<img width="193" height="106" alt="Screenshot 2026-06-25 203603" src="https://github.com/user-attachments/assets/f81de28c-5316-4884-bc28-e1d7188ab6a5" />

```
podman compose up -d                                                                                                                                  
ls                                                                                                                                                    
nvim docker-compose.yml  
```

<img width="203" height="128" alt="Screenshot 2026-06-25 203845" src="https://github.com/user-attachments/assets/5b310a19-97c4-4ea1-b642-a1505254fc91" />


