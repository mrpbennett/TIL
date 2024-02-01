# Setting a static IP

First we need to get information about our network adapters.

```bash
ip a


1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet xxx.0.0.x/x scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    inet 192.xxx.x.xxx/22 metric 100 brd 192.xxx.x.xxx scope global dynamic ens18
       valid_lft 14340sec preferred_lft 14340sec
    inet6 xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx scope global dynamic mngtmpaddr noprefixroute
       valid_lft 1741sec preferred_lft 1741sec
    inet6 xx:xx:xx:xx:xx:xx scope link
       valid_lft forever preferred_lft forever
```

My current network adapter is `ens18` It has `22` bits reserved for the netmask.

Time to find out what we can about the subnet

```bash
ifconfig a


ens18: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.xxx.x.113  netmask xxx.xxx.xxx.0  broadcast 192.xxx.x.xxx
```

Now we have the relevant information we need to edit our netplan file that lives in `/etc/netplan`

Now copying this template we need to make our changes

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses:
        - 10.10.10.2/24
      nameservers:
        search: [mydomain, otherdomain]
        addresses: [10.10.10.1, 1.1.1.1]
      routes:
        - to: default
          via: 10.10.10.1
```
