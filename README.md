# linux-networking

show ip
```bash
ip addr show ens3 | grep inet
```

show IP routing table
```bash
route -n
```

show routing system:
```bash
ip route show
```

add alternate name for a network interface:
```bash
ip link property add wlp0s20f3 altname eth0
```
Check all network devices status:
```
nmcli -p dev
```

### TCP sockets

List all Open TCP sockets connections:
```bash
netstat -nplt
```

Check number of connections:
```bash
netstat -anp | grep etcd
```

### Network Namespace

create two network namespaces `red` and `blue`
```bash
ip netns add red
ip netns add blue
```

check the namespaces
```bash
tree /var/run/netns/
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

### link two networks

create virtual networks for red and blue namespaces
```bash
ip link add veth-red type veth peer name veth-blue
```

delete ip link network
```bash
ip -n red link del veth-red
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

check the networkn ip
```bash
ip netns exec red ip address show
ip netns exec blue ip address show
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
---

### bridge

create a bridge
```bash
ip link add v-net-0 type bridge
```

start the bridge
```bash
ip link set dev v-net-0 up
```

create network link for bridge
```bash
ip link add veth-red type veth peer name veth-red-br
ip link add veth-blue type veth peer name veth-blue-br
```

link the network `red` to namespaces and bridge
```bash
ip link set veth-red netns red
ip link set veth-red-br master v-net-0
```

link the network `blue` to namespaces and bridge
```bash
ip link set veth-blue netns blue
ip link set veth-blue-br master v-net-0
```

assign IPs to namespaces
```bash
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue
```

start the networks
```bash
ip -n red link set veth-red up
ip -n blue link set veth-blue up
```
