# netns


check all ip links:
```bash
ip link
```

become root user
```bash
sudo su
```

create two links `tap1` and `tap2` and connect them via `veth`
```bash
ip link add tap1 type veth peer name tap2
```

create two network namespaces `red` and `blue`
```bash
ip netns add red
ip netns add blue
```

move links to network namespaces
```bash
ip link set tap1 netns red
ip link set tap2 netns blue
```

check networks inside `red` and `blue` network namespaces:
```bash
ip netns exec red ip a
ip netns exec blue ip a
```

start the `tap1` network and set an ip
```bash
ip netns exec red ip link set tap1 up
ip netns exec red ifconfig tap1 192.168.1.2/24
# now check again
ip netns exec red ip a
```

start the `tap2` network and set an ip
```bash
ip netns exec blue ip link set tap2 up
ip netns exec blue ifconfig tap2 192.168.1.3/24
# now check again
ip netns exec blue ip a
```

try to ping from one network to other:
```bash
ip netns exec red ping 192.168.1.3
ip netns exec blue ping 192.168.1.2
```

### Cleanup

```bash
ip netns delete red
ip netns delete blue
```
