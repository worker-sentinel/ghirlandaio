# HOW TO INSTALL OS SERVER 1

## 1. Cek partisi
```
lsblk
```
## 2. Physical volume
```
pvcreate /dev/sda6
```
## 3. Volume group
```
vgcreate server1 /dev/sda6
```
## 4. Logical volume
```
lvcreate -L 5G server1 -n root
lvcreate -L 5G server1 -n vars
lvcreate -L 1G server1 -n vlog
lvcreate -L 1G server1 -n vaud
lvcreate -L 1.5G server1 -n vtmp
lvcreate -L 9G server1 -n podman
```
```
lsblk
```
```
lvcreate 
 

