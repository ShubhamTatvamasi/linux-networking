# brctl

Install brctl:
```bash
sudo apt install bridge-utils
```

create a bridge network:
```bash
sudo brctl addbr br0
```

add interface to bridge:
```bash
sudo brctl addif br0 wlp0s20f3
```




delete bridge network:
```bash
sudo brctl delbr br0
```

