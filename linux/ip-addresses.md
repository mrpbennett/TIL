# Checking VM IP address

To check the IP address of your VM you can run the the `ip addr show` command
which will producve the following or something similar:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:56:a8:c8:4c brd ff:ff:ff:ff:ff:ff
    inet 10.201.24.179/22 brd 10.201.27.255 scope global eth0
       valid_lft forever preferred_lft forever
```

- `lo`: is the loop back address.
- `eth0`: is the IP we would need to connect to the virtual machine

If the IP address was not set within the VM you're able to set it by running:

```bash
ip addr add 123.123.1.20/24 dev eth0
```

This will set the IP address for `eth0` allowing you to connect via SSH
