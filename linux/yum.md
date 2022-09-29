# Yum basics

YUM is the package manager for [CentOS](https://www.centos.org)

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
