# 90 Days of DevOps

A list of videos / courses I have completed and my learnings.

## Prerequisites

### [DevOps Prerequisites Course - Getting started with DevOps](https://youtu.be/Wvf0mBNGjXY)

#### Things learnt I this video

#### Linux basics

- `echo` - prints to screen
- `ls` - lists files and folders
- `cd` - change directory
- `pwd` - prints working directory
- `mkdir` - makes a directory
- `hostname` - prints the hostname

#### Directories

- `cd new-dir; mkdir www; pwd` - changes to `new-dir` creates dir `www` and
  prints working directory
- `mkdir -p /tmp/hello/world` - creates whole dir stucture the `-p` flag means:
  _“create the parent directories, too”_
- `rm -r /tmp/my-dir` - removes the directory the `-r` flag means: _to work
  recursively_
- `cp -r my-dir /tmp/my-dir` - copy directory to a new location `-r` allows
  copying of whole directory

#### Files

- `touch <filename>` - creates blank file
- `cat > test.txt` - allows you to add contents to file press `ctl-d` to
  exit...better to use vim IMO.
- `cat test.txt` - shows you contents of file
- `cp test.txt backup-test.txt` - copies the contents of file and creates a new
  one `<orignal> <copy>`
- `mv test.txt new-test.txt` - renames the file (unless specifying a dir)

#### User accounts

- `whoami` - prints our current user to the terminal
- `id` - prints out more information about current user
- `su <user>` - switch to a different user
- `ssh <user>@123.123.123.0.0` - ssh into a server as a different user

#### Download files

- `curl <some url> -O` - downloads a file using curl the `-O` flags allows the
  saving of file.
- `wget <some url> -O <file name>` - same as above but allows you to name it.

#### Check OS version

- `ls /etc/*release*` - inspects the release files
- `cat /etc/*release*` - allows more info

### Package managers

#### RPM (RedHat package manager)

rpm doesnt install dependencies

- `rpm -i someapp.rpm` - installs package
- `rpm -e someapp.rpm` - uninstalls package
- `rpm -q someapp.rpm` - queries package

#### YUM

- `yum install someapp` - YUM will install `someapp` and all it's dependencies
- `yum repolist` - lists all the yum repos example output below:

```
repo id                                                repo name                                                               status
VMware-tools-collection                                VMware-tools-collection                                                     41
base/7/x86_64                                          CentOS-7 - Base                                                         10,072
docker_ce/x86_64                                       Docker CE Stable - x86_64                                                  169
elastic-7.x                                            Elastic repository for 7.x packages                                      1,404
epel/x86_64                                            Extra Packages for Enterprise Linux 7 - x86_64                          13,752
extras/7/x86_64                                        CentOS-7 - Extras                                                          516
!icinga-stable-release/7                               icinga-stable-release                                                    1,309
!icinga-stable-release-tests/7                         icinga-stable-release-tests                                              1,309
!pulsepoint                                            PulsePoint                                                               2,312
puppet6/x86_64                                         Puppet 6 Repository el 7 - x86_64                                          358
rsyslogall/7/x86_64                                    rsyslog                                                                  3,075
trivy/7/x86_64                                         Trivy repository                                                           100
updates/7/x86_64                                       CentOS-7 - Updates                                                       4,244
```

- `ls /etc/yum.repos.d` - lists all the repos and their config files
- `cat /etc/yum.repos.d/<repo file>` - shows the url of where the packages of
  repo are install - example output:

```
[docker_ce]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/7/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

- `yum list` - shows all installed packages
- `yum remove <package>` - removes package
- `yum --showduplicates list <package>` - lists all avaiable packages for
  `<package>`
- `yum install <package>.0.0.1` - installs a package from a certain version

### Services

- `service <service> start` - will start a service
- `service <service> status` - will give the status of the service

## Linux networking

Hooking the VM to your DNS server, use the following for example. Adding this in
the host.

```
cat /etc/resolv.conf

nameserver 192.168.1.100
```

Giving the ability to ping subdomain urls by their subdomain name. For example
if were to have `web.company.com` but you wanted to ping this by using
`ping web` it would fail.

To resolve this add `search company.com` into your `/etc/resolv.conf` file on
the host.

| nslookup option         | description                                                  |
| :---------------------- | :----------------------------------------------------------- |
| `-domain=[domain name]` | Change the default DNS name.                                 |
| `-debug`                | Show debugging information.                                  |
| `-port=[port-number]`   | Specify the port for queries. The default port number is 53. |
| `-timeout=[seconds]`    | Specify the time allowed for the server to respond.          |
| `-type=a`               | View information about the DNS A address records.            |
| `-type=any`             | View all available records.                                  |
| `-type=hinfo`           | View hardware-related information about the host.            |
| `-type=mx`              | View Mail Exchange server information.                       |
| `-type=ns`              | View Name Server records.                                    |
| `-type=ptr`             | View Pointer records. Used in reverse DNS lookups.           |
| `-type=soa`             | View Start of Authority records.                             |

The `dig <some ip>` will give you more information on that IP address

---

Running the `route` command will display the Kernels latest routing table and
allow you to see the current routing config

To add a gateway from one network to another you can run the `ip route add`
command for example:

```bash
Network 1 = 192.168.1.0
Network 2 = 192.168.2.0

ip route add 192.168.2.0/24 via 192.168.1.1
```

Will allow network 2 to speak to network 1 via the gateway on network 1

For any network you don't know a route too you can use is address as the default
gatwway

`ip route add default via 192.168.2.1`: for example

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.2.1     0.0.0.0         UG    0      0        0 eth0
```

You could also use `0.0.0.0` instead of `default` which means any IP
destination. Having `0.0.0.0` as a gateway means it does not require one as its
on an internal network.

| ip command           | description                                    |
| :------------------- | :--------------------------------------------- |
| `ip`                 | List and modify interfaces on the host         |
| `ip addr`            | See the IP addresses asigned to the interfaces |
| `ip route` / `route` | Provides the routing table                     |

All changes with above commands are only valid till a reboot. These would have
to be set in certain files like `/etc/sysctl.conf` for forwarding.

---

### CCNA

A network is considred to be a group of IP addresses, where a router is needed
to connect different networks. Or a switch to extend the items in the current
network.

#### Switches

- Layer 1 addresses are subject to change, anything physically plugged in
- Layer 2 are mac addresses these will **never** change

Get access to the mac address table on a Cisco switch

```bash
Switch> enable
Switch# show mac-address-table
```

- Q: Which of the following addresses will a switch use to populate a CAM table
- A: source MAC address

A switch will use source Media Access Control (MAC) addresses to populate the
Content Addressable Memory (CAM) table. The CAM table, which is also called the
'switching table' is used by a switch to discorver the relationship between the
Open Systems Interconnection (OSI) Layer 2 addres of a device and the physical
port used to reach the device. The switch populates the CAM table by recording
the source MAC address of an inbound Layer 2 frame and the corresponding switch
port that the frame arrived on.

- Q: Which of the following addresses will a switch use to make forwarding
  decisions?
- A: destination MAC address

A switch will use destination MAC addresses to make forwarding descision.
Swiches make forwarding decisions base on the destintion MAC address contained
in the frame's header. The swich first searches the CAM table for an entry that
maches the frame's destination MAC address. The CAM table, which is also called
the switching table, used by a switch to discover the relationship between Layer
2 address of a device and the physical port used to reach the device. If the
frame's desitnation MAC address is not found in the table, the switch forwards
the frame to all its ports. except the port from which it recieved the frame. If
the destination MAC address is found in the table, the switch forwards the frame
to the appropriate port. The source MAC address is also recorded if it did not
previously exist in the CAM table.

#### Routers

Routers are designed to connect networks together.

#### TCP/IP - OSI

See more [here](ccna/../../ccna/tcpip-osi.md)

##### TCP/IP Model

|   TCP/IP    | description                                                        |
| :---------: | :----------------------------------------------------------------- |
| Application | Layer 7 - Interaction layer where apps can access network services |
|  Transport  | Transmits data using TCP / UDP protcol                             |
|   Network   | Layer 3 - IP addresses assigned                                    |
|  Data Link  | Layer 2 - MAC addresses                                            |
|  Physical   | Layer 1 - Eth cables / NIC / Physical inputs                       |

##### OSI Model

|    TCP/IP    | description                                                        |
| :----------: | :----------------------------------------------------------------- |
| Application  | Layer 7 - Interaction layer where apps can access network services |
| Presentation | Ensures data is in a usable format                                 |
|   Session    | Maintains connections controllong ports / sessions                 |
|  Transport   | Transmits data using TCP / UDP protcol                             |
|   Network    | Layer 3 - IP addresses assigned                                    |
|  Data Link   | Layer 2 - MAC addresses                                            |
|   Physical   | Layer 1 - Eth cables / NIC / Physical inputs                       |

Remembering the OSI model: **All People Seem To Need Data Processing** /
**Please Do Not Throw Sausage Pizza Away**

- Q: Which of the following operates primarily at Layer 2 on the OSI model?
- A: a switch

A switch operates primarily at Layer 2 of the OSI mode. Switches operator on the
Data Link layer. Where as a router operators on a Layer 3

- Q: Which of the following operate primarily at the Physical Layer of the OSI
  model?
- A: repeaters & hubs

###### What connects to what...

|  Device  | description |
| :------: | :---------- |
| Switches | Layer 2     |
| Routers  | Layer 3     |
|   Hubs   | Layer 1     |
|          |             |
|          |             |

- Prerequisites Introduction to Linux - https://www.edx.org/course/introducti...
- DevOps Prerequisite (freecodecamp) - https://youtu.be/Wvf0mBNGjXY ✅
- CCNA part 1-https://youtu.be/rv3QK2UquxM
- Golang complete course (techworldwithnana) -https://youtu.be/yyUHQIec83I
- YAML Introduction to YAML - https://youtu.be/1uFVr15xDGg

### Git

- git Git and GitHub crash course (freecodecamp) - https://youtu.be/RGOj5yH7evk
  ✅
- Complete Git and GitHub Tutorial (Kunal Kushwaha) -
  https://youtu.be/apGV9Kg7ics
- Git for Professionals (freecodecamp) -https://youtu.be/Uszj_k0DGsg
- cloud AWS cloud practitioner (freecodecamp) - https://youtu.be/SOTamWNgDKc

### Docker

- docker Docker playlist by Saloni - https://youtube.com/playlist?list=PL5...
- Docker ((freecodecamp) - https://youtu.be/9zUHg7xjIqQ
- Docker (Techworldwithnana) - https://youtu.be/3c-iBn73dDE
- Docker deep dive by Nigel - https://youtu.be/GwXLNAcHk-k
- Dockerfile best practices - https://youtu.be/JofsaZ3H1qM
- Docker security essentials - https://youtu.be/KINjI1tlo2w
- Auditing Docker security - https://youtu.be/mQkVB6KMHCg
- Docker in a visual way - https://aurelievache.gumroad.com/l/un...
- Ivan container articles - https://iximiuz.com/en/categories/?ca...

### Kubernetes

- Kubernetes Civo Academy - civo.com/academy
- Kubernetes course (techworldwithnana) - https://youtu.be/X48VuDVv0do
- Kube academy -https://kube.academy/
- Introduction to Kubernetes (edx) - https://www.edx.org/course/introducti...
- KCNA - https://youtu.be/iGkFHB1kFZ0
- Hands on CKA/CKAD/CKS - https://youtu.be/jZOs8Oips7Q
- Certs Magic show -https://youtube.com/playlist?list=PLj...
- CKS book -https://saiyampathak.gumroad.com/l/ck...

### CI/CD

- Jenkins complete course - https://youtu.be/FX322RVNGj4
- Github actions (techworldwithnana) - https://youtu.be/R8_veQiYBjI
- GitHub actions with cloud run - https://youtu.be/eooi60Mks_0
- CI/CD week - https://youtube.com/playlist?list=PL5...
- Get Certified for GitOps with Argo - https://codefresh.learnworlds.com/

### IAC tools

- Terrafrom in 2 hours (freecodecamp) - https://youtu.be/SLB_c_ayRMo
- Hashicorp terraform accociate certification
  (freecodecamp) -https://youtu.be/V4waklkBC38
- Crossplane CNCFMinutes -https://youtu.be/NLHmqVUvtkU
- Crossplane deep dive - https://youtu.be/5lWUWat_bbY
- Crossplane composition deepdive - https://youtu.be/78xR7ypzB4Q
- Learn pulumi -https://youtu.be/vIjeiDcsR3Q

### Observability

- Getting started with Jaeger - https://youtu.be/aMZoUIG-mgY
- Getting dirty with Monitoring and Autoscaling Features for Self Managed
- Kubernetes cluster - https://youtu.be/TqfIfUuuPdE
- Intro to Kubernetes monitoring - https://youtu.be/B5UY-qeW96I
- Prometheus CNCFMinutes - https://youtu.be/llwxJ0VdYWY
- Thanos CNCFMinutes - https://youtu.be/Pr3MbsGHljI
- Thanos deep dive - https://youtu.be/nYV_wU7_Xm0

### Chaos engineering

- Chaos mesh CNCFMinutes - https://youtu.be/HAU_cjW1bMw
- Chaos mesh 2.0 - https://youtu.be/HmQ9cFwxF7g
- Litmus CNCFMinutes -https://youtu.be/rDQ9XKbSJIc
- Cloud native chaos paradigms -https://youtu.be/uBGPFfTu6TU

### Policy engines

- Kyverno CNCFMinutes - https://youtu.be/Bo8KhWhNY6g
- Kyverno deep dive - https://youtu.be/QR-iBeh9Vy0
- Kyverno courses -https://learn.nirmata.com/courses/
- jsPolicy - https://youtu.be/AzPczzKW71A
- Kubewarden - https://youtu.be/b14YkyrLFcs
- OPA CNCFMinutes -https://youtu.be/49my68py3KY
- OPA deep dive - https://youtu.be/J6tM9O-0LvI
- Various policy engines for Kubernetes - https://youtu.be/gKQOq7904h8 Styra
- Academy - https://academy.styra.com/courses/opa...

### Servicemesh

- Introduction to Service mesh with Linkerd -
  https://www.edx.org/course/introducti...
