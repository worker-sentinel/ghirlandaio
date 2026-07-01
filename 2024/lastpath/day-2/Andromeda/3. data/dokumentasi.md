## Connect wifi satu jaringan
```
iwctl
```
station wlan0 connect [nama wifi]
```
```
exit
```

## Connect kubernetes ke admin
```
curl -sfL https://get.k3s.io | sudo sh -s - agent --server https://10.237.160.246:6443 --token [token admin]
```
```
pacman -S kubectl
```


