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

check arp table inside and outside network namespace
```bash
arp
ip netns exec red arp
```




