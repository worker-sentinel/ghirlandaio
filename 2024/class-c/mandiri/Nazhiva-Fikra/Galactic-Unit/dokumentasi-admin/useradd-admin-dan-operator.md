# Membuat user admin
```
useradd -m admingalactic
```
```
passwd admingalactic
```
```
echo "admingalactic ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
```

# Membuat user operator
```
useradd -m operatorgalactic
```
```
passwd operatorgalactic
```
```
exit
```
