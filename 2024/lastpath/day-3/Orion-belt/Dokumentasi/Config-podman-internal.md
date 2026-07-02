# Internal

Config Podman

```
sudo nvim /etc/containers/registries.conf[[]]
```

masuk menu insert (i) lalu setelah itu edit 
 hapus hastag pada bagian unquallfied-search-registries =  [example.com] 
 ganti example.com menjadi
```
unquallfied-search-registries = [docker.io] 
```

```
sudo systemctl enable podman
```


```
sudo systemctl start podman
```


cek ip

```
ip a
```

```
systemctl status k3s-agent.service
```

```
hwclock --show
```


