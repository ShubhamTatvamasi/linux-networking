# default route

add default route:
```bash
ip route add default via 1.1.1.1 dev enp2s0
```
> change the ip with your public ip

update config:
```bash
sudo vim /etc/netplan/00-installer-config.yaml
```
```yaml
network:
  version: 2
  ethernets:
    enp2s0:
      dhcp4: true
      routes:
      - to: 0.0.0.0/0
        via: 1.1.1.1
```
> update the `via` IP with Gateway IP
