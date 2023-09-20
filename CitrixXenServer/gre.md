# Using GRE tunnel
❗️ Can use it just when 2 servers commuticated direcly

## server1
**iptables**
```
-A INPUT -p gre -j ACCEPT
-A RH-Firewall-1-INPUT -i gre-temp -m conntrack --ctstate NEW -j ACCEPT
```

**kernel modules**
```shell
modprobe ip_gre
```

**ip link**
```shell
iptunnel add gre-temp mode gre local $SERVER1_IP remote $SERVER2_IP ttl 225
ip a add 10.10.10.1/30 dev gre-temp
ip link set gre-temp up
```

**temp web-server**
```shell
cd /mnt/$DIR
python -m SimpleHTTPServer
```

## server-2

**kernel modules**
```shell
modprobe ip_gre
```

**ip link**
```shell
iptunnel add gre-temp mode gre remote $SERVER1_IP local $SERVER2_IP ttl 225
ip a add 10.10.10.2/30 dev gre-temp
ip link set gre-temp up
```

Now you can use `wget` for quick transferring date via gre tunnel
