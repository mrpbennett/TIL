# Installing HA Kubernetes (k3s)

I have always wanted to learn how to install and manage a Kubernetes cluster so this is how I have gone about it. After going through many tutorials, YT videos and even the Kubernetes official docs. I struggled to get a cluster up and running. The official docs just IMO seem all over the place.

Enter [K3S](https://k3s.io/) a certified Kubernetes distribution built for IoT & Edge computing.

Below is how I installed my HA cluster, with K3s and HAProxy. It may not be perfect but it's up and running for me.

## Virtual Machines

First let's create some virtual machines, after reading the k3s docs about HA and for me wanting to follow the instructions to a tee I went ahead and created the following virtual machines.

! note: the IP addresses are for example only.

#### K3s servers

- `k3s-svr-1`: 3 core / 6gb - `192.168.10.121`
- `k3s-svr-2`: 3 core / 6gb - `192.168.10.122`
- `k3s-svr-3`: 3 core / 6gb - `192.168.10.123`

#### Load balancers

- `k3s-lb-1`: 2 core / 2gb - `192.168.8.101`
- `k3s-lb-2`: 2 core / 2gb - `192.168.8.102`

#### K3s agents

- `k3s-agt-1`: 2 core / 2gb - `192.168.10.111`
- `k3s-agt-2`: 2 core / 2gb - `192.168.10.112`
- `k3s-agt-3`: 2 core / 2gb - `192.168.10.113`
- `k3s-agt-4`: 2 core / 2gb - `192.168.10.114`

## Tweaking our VMs to have static IPs

Now we have all our VMs up and running it's time to update them and give them static IPs. The easiest way to update them would be to run `sudo apt update -y && sudo apt upgrade -y` once that's done let's set our static IPs.

Head over to the `netplan` directory so we can edit the file inside like so: `sudo nano /etc/netplan/your_config_file.yaml`

Go ahead and copy this template and make changes where needed.

```yaml
# This is the network config written by 'subiquity'
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      addresses:
        - 192.168.x.xxx/22 # the IP you want to be static
      routes:
        - to: default
          via: 192.168.x.x # router ip
      nameservers:
        addresses: [192.168.x.x, 1.1.1.1] # DNS / cloudflare
```

When applying the new config I kept coming across the following error:

```bash
** (generate:2496): WARNING **: 05:24:24.943: Permissions for /etc/netplan/my-config-file.yaml are too open. Netplan configuration should NOT be accessible by others.
```

This was resolved after searching the internet, I [found the following](https://askubuntu.com/questions/1477287/need-help-with-a-netplan-configuration-issue) which suggested setting different permissions to the file like so:

```bash
sudo chmod 600 /etc/netplan/your_config_file.yaml
```

Which removed the error, and I was able to go ahead and push the new config iva `sudo netplan apply` and then reboot the VM with `sudo reboot`

## Setting up Load balancers

Now our all VMs have static IPs and have been rebooted. We can start setting up our load balancers. This is what I started to struggle with in the K3s docs. I found the document structure to be a little confusing.

As it goes like this:

- [Backup and Restore](https://docs.k3s.io/datastore/backup-restore)
- [High Availability Embedded etcd](https://docs.k3s.io/datastore/ha-embedded)
- [High Availability External DB](https://docs.k3s.io/datastore/ha)
- [Cluster Load Balancers](https://docs.k3s.io/datastore/cluster-loadbalancer)

I was going down the list, which meant the load balancer was last. But if you head to the load balancer page it clearly states:

> For both examples, assume that a HA K3s cluster with embedded etcd has been installed on 3 nodes.

So after some playing around, reading the docs more than once, and reading through Techno Tims post about [HIGH AVAILABILITY k3s (Kubernetes) in minutes!](https://technotim.live/posts/k3s-ha-install/). Where he mentions creating a load balancer first and using the IP of the load balancer on `tls-san`. I went and created the Load Balancers first.

Most of the tutorials I have seen have gone with [Nginx](https://docs.nginx.com/), but the k3s docs clearly state:

> Nginx does not natively support a High Availability (HA) configuration. If setting up an HA cluster, having a single load balancer in front of K3s will reintroduce a single point of failure.

So I decided against it and went with [HAProxy](http://www.haproxy.org/) here is how I set things up.

1. Install HAProxy and KeepAlived:

`ssh` into both your load balancers and run `sudo apt-get install haproxy keepalived` this will install the applications we need.

2. Add the following to `/etc/haproxy/haproxy.cfg` on k3s-lb-1 and k3s-lb-2:

```bash
frontend k3s-frontend
    bind *:6443
    mode tcp
    option tcplog
    default_backend k3s-backend

backend k3s-backend
    mode tcp
    option tcp-check
    balance roundrobin
    default-server inter 10s downinter 5s
    server k3s-svr-1 192.168.10.121:6443 check
    server k3s-svr-2 192.168.10.122:6443 check
    server k3s-svr-2 192.168.10.123:6443 check
```

3. Add the following to `/etc/keepalived/keepalived.conf` on k3s-lb-1 and k3s-lb-2:

```bash
vrrp_script chk_haproxy {
    script 'killall -0 haproxy' # faster than pidof
    interval 2
}

vrrp_instance haproxy-vip {
   interface eth1
    state <STATE> # MASTER on lb-1, BACKUP on lb-2
    priority <PRIORITY> # 200 on lb-1, 100 on lb-2

    virtual_router_id 51

    virtual_ipaddress {
        192.168.10.100/24 # This is your Virtual IP Address
    }

    track_script {
        chk_haproxy
    }
}
```

4. Restart HAProxy and KeepAlived on k3s-lb-1 and k3s-lb-2:

```bash
systemctl restart haproxy
systemctl restart keepalived
```

Now this was the first time setting up HAProxy, actually, it was the first time setting up a load balance entirely. So I learnt a few things along the way after asking ChatGPT. This is what I learnt.

### What is `tls-san`

The tls-san flag is used in Kubernetes and k3s to specify additional Subject Alternative Names (SANs) for the TLS certificates used by the Kubernetes API server. This is important for ensuring that the certificates are valid for all the names or IP addresses that might be used to access the API server

### Why does the k3s docs use `10.10.10.100` when setting up `tls-san`

Using the docs I asked the following in ChatGPT

If I have two load balancer IPs like

```
lb-1: 10.10.10.98
lb-2: 10.10.10.99
```

Why do I have a `tls-san: 10.10.10.100` when using haproxy

```bash
vrrp_script chk_haproxy {
    script 'killall -0 haproxy' # faster than pidof
    interval 2
}

vrrp_instance haproxy-vip {
   interface eth1
    state <STATE> # MASTER on lb-1, BACKUP on lb-2
    priority <PRIORITY> # 200 on lb-1, 100 on lb-2

    virtual_router_id 51

    virtual_ipaddress {
        10.10.10.100/24
    }

    track_script {
        chk_haproxy
    }
}
```

The answer then made everything crystal clear to me:

In your HAProxy configuration with KeepAlived, `10.10.10.100` is set up as a virtual IP address (VIP) that is shared between the two load balancers (`lb-1` and `lb-2`). This VIP is used to provide a single, consistent entry point for your cluster, even though there are two physical load balancers.

The tls-san flag in this context is used to add `10.10.10.100` as a Subject Alternative Name in the TLS certificate for the Kubernetes API server. This ensures that when clients connect to the API server using the VIP (`10.10.10.100`), the TLS certificate is recognized as valid. Without this, clients connecting to the Kubernetes API server through the VIP would receive a certificate error, as the certificate wouldn't be valid for this IP.

So, even though your load balancers have different IPs (`10.10.10.98` and `10.10.10.99`), the traffic is actually routed through the VIP (`10.10.10.100`), and this is why the `tls-san` flag includes the VIP.

I then of course converted all those IP addresses to my own, as above.

### What should I use for as the `tls-san` when creating my server

I asked:

now should the tls-san here

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --cluster-init \
    --tls-san=<FIXED_IP> # Optional, needed if using a fixed registration address
```

Be the IP of the node or the VIP from the 2 load balancers?

**Answer**

In the context of your k3s cluster setup with a VIP configured through HAProxy and KeepAlived, the `--tls-san` parameter should be set to the VIP of the load balancers, not the IP of the individual node. This ensures that the TLS certificate is valid for the VIP (`10.10.10.100` in your case), which is the address used by clients to access the cluster. By doing this, you ensure that connections to the Kubernetes API server through the VIP are secure and trusted by the clients.

## Setting up our cluster

We're going to create a High Availability Embedded etcd cluster. I tried an external data source by running a Docker instance of Postgres, my docker instance ran out of space so decided to go down this route instead.

> HA embedded etcd cluster must be comprised of an odd number of server nodes for etcd to maintain quorum. For a cluster with n servers, quorum is (n/2)+1. For any odd-sized cluster, adding one node will always increase the number of nodes necessary for quorum. Although adding a node to an odd-sized cluster appears better since there are more machines, the fault tolerance is worse since exactly the same number of nodes may fail without losing quorum but there are more nodes that can fail.

An HA K3s cluster with embedded etcd is composed of:

- Three or more server nodes that will serve the Kubernetes API and run other control plane services, as well as host the embedded etcd datastore.
- Optional: Zero or more agent nodes that are designated to run your apps and services
- Optional: A fixed registration address for agent nodes to register with the cluster

`ssh` into your k3s servers and configure the server with the following:

```bash
# /etc/rancher/k3s/config.yaml
token: 39f504e2480a6c506785a211f49149c4
tls-san: 192.168.10.100
```

For the token this is going to be used as the secret to join our servers and agents into the cluster. To create one open up and terminal and run `openssl rand -hex 16` and save that for later.

Now we have the `/etc/rancher/k3s/config.yaml` configured let's concentrate on launching our first launch server node with the `cluster-init` flag to enable clustering and a token that will be used as a shared secret to join additional servers to the cluster.

I have used an extra flag `--node-taint CriticalAddonsOnly=true:NoExecute` here to prevent any pods from coming on to the servers. This lets the agents handle all the pods and our servers do what they do best. For the `K3S_TOKEN` let's use the one above or the one you created with `openssl rand -hex 16`

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=39f504e2480a6c506785a211f49149c4 sh -s - server \
 --cluster-init \
 --node-taint CriticalAddonsOnly=true:NoExecute \
 --tls-san=192.168.10.100 # vip from the load balancer
```

After launching the first server, join the second and third servers to the cluster using the shared secret. Here the `--server` will be the IP of the first server node you created:

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=39f504e2480a6c506785a211f49149c4 sh -s - server \
 --server https://192.168.10.110:6443 \
 --node-taint CriticalAddonsOnly=true:NoExecute \
 --tls-san=192.168.10.100 # vip from the load balancer
```

Check to see that the second and third servers are now part of the cluster:

```bash
$ kubectl get nodes
NAME        STATUS   ROLES                       AGE   VERSION
k3s-svr-1     Ready    control-plane,etcd,master   28m   vX.Y.Z
k3s-svr-2     Ready    control-plane,etcd,master   13m   vX.Y.Z
k3s-svr-3     Ready    control-plane,etcd,master   10m   vX.Y.Z
```

Now you have a highly available control plane. Any successfully clustered servers can be used in the `--server` argument to join additional server and agent nodes. Joining additional agent nodes to the cluster follows the same procedure as servers:

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=39f504e2480a6c506785a211f49149c4 sh -s - agent --server https://192.168.10.100:6443 # vip from load balancer
```

Now let's check if our agents have joined our cluster.

```bash
$ kubectl get nodes
NAME        STATUS   ROLES                       AGE   VERSION
k3s-svr-1     Ready    control-plane,etcd,master   28m   vX.Y.Z
k3s-svr-2     Ready    control-plane,etcd,master   13m   vX.Y.Z
k3s-svr-3     Ready    control-plane,etcd,master   10m   vX.Y.Z
k3s-svr-3     Ready    control-plane,etcd,master   10m   vX.Y.Z
k3s-agt-1     Ready    <none>                       8m   vX.Y.Z
k3s-agt-2     Ready    <none>                       8m   vX.Y.Z
k3s-agt-3     Ready    <none>                       8m   vX.Y.Z
k3s-agt-4     Ready    <none>                       8m   vX.Y.Z
```

There we have it...a Kubernetes cluster. Before this, I was ready to throw my Proxmox nodes out the window. But then I found k3s and realised after spending some time sitting down it wasn't too hard.

I hope this has helped at least one person and saved them the headaches I had.
