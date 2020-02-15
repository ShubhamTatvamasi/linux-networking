# linux-networking

### Network Namespace

create two network namespaces `red` and `blue`
```bash
ip netns add red
ip netns add blue
```

open a shell inside network namespace
```bash
ip netns exec red bash
```

check network namespaces 
```bash
ip netns
```

check ip links
```bash
ip link
```

check ip links inside `red` namespace
```bash
ip netns exec red ip link
ip -n red link
```

check arp table outside and inside network namespace
```bash
arp # outside
ip netns exec red arp # inside
```

check route available outside and inside network namespace
```bash
route # outside
ip netns exec red route # inside
```
---

create virtual networks for red and blue namespaces
```bash
ip link add veth-red type veth peer name veth-blue
```

link the network to namespaces
```bash
ip link set veth-red netns red
ip link set veth-blue netns blue
```

assign IPs to namespaces
```bash
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue
```

start the network
```bash
ip -n red link set veth-red up
ip -n blue link set veth-blue up
```

ping from red network to blue network
```bash
ip netns exec red ping 192.168.15.2
```



