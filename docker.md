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


Source: https://github.com/nicolaka/netshoot

