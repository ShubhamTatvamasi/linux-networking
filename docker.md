# docker

start netshhot container for network debugging:
```bash
docker run --rm -it nicolaka/netshoot sh
```

list ip routes:
```bash
docker run --rm -it nicolaka/netshoot ip route list
```

start a sleep container:
```bash
docker run -d --name netshoot nicolaka/netshoot sleep infinity

# get the process ID
NETSHOOT_PROCESS_ID=`docker inspect netshoot -f '{{.State.Pid}}'`
sudo ps aux | grep $NETSHOOT_PROCESS_ID

# check network interfaces inside docker container
sudo nsenter -t $NETSHOOT_PROCESS_ID -n ip a
sudo nsenter -t $NETSHOOT_PROCESS_ID -n ip route list
```

check namespace:
```bash
sudo lsns --type net
```

run docker container in `host` or `none` network:
```bash
docker run --rm -it --network host nicolaka/netshoot sh
docker run --rm -it --network none nicolaka/netshoot sh
```

check iptables for all network:
```bash
sudo iptables -t nat -L -v
```

test on kubernetes:
```bash
kubectl run --rm -it --image=nicolaka/netshoot -- sh
```
Source: https://github.com/nicolaka/netshoot

