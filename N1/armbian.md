### 系统如何设置固定ip
By default your main network adapter's IP is assigned by your router DHCP server.

Edit /etc/network/interfaces and change from:
```conf
iface eth0 inet dhcp
```

to - for example:
```conf
iface eth0 inet static
address 192.168.1.100
netmask 255.255.255.0
gateway 192.168.1.1
```
