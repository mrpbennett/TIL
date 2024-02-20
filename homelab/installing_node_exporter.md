# Installing Node exporter

The Prometheus [Node Exporter](https://github.com/prometheus/node_exporter) exposes a wide variety of hardware- and kernel-related metrics. This has to be installed on every host you wish to monitor, a lot of tutorials mainly put it on a Docker host, which only monitors the Docker host node.

What if you want to monitor all your servers in your homelab. Here we will install `node_exporter` and start it as a service in Ubuntu.

## Installing and running the Node Exporter

The Prometheus Node Exporter is a single static binary that you can install via tarball. Once you've downloaded it from the Prometheus [downloads page](https://prometheus.io/download#node_exporter) extract it, and run it.

So if we were to go at to the downloads page at the time of writing this, the latest version is `1.7.0` and we want the `linux-amd64` install. Therefore our below would be.

- First download the file to your `$HOME` dir
- Create a `node_exporter` directory within your `/etc` dir for safekeeping
- `cd` into the new directory
- Extract the `node_exporter` file
- `cd` into the `node_exporter` directory
- run `./node_exporter` to get logs

```bash
# NOTE: Replace the URL with one from the above-mentioned "downloads" page.
# <VERSION>, <OS>, and <ARCH> are placeholders.
# wget https://github.com/prometheus/node_exporter/releases/download/v<VERSION>/node_exporter-<VERSION>.<OS>-<ARCH>.tar.gz

wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz

sudo mkdir /etc/node_exporter

cd /etc/node_exporter

tar xvfz $HOME/node_exporter-1.7.0.linux-amd64.tar.gz

cd node_exporter-1.7.0.linux-amd64

./node_exporter
```

Now this is mainly it, ...but every time we reboot the service won't start and we would need to start the `node_exporter` again. So let's create a service to start on boot.

## Creating the service

I am installing this on my Proxmox node so we're doing this as `root` your command may differ, but you want to be placing your file within the `systemd/system` directory. Mine is `lib/systemd/system/`. Let's create the system file.

```bash
nano /lib/systemd/system/node_exporter.service
```

Add the following configuration

```bash
[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
ExecStart=/etc/node_exporter/node_exporter-1.7.0.linux-amd64/node_exporter \
  --web.listen-address=:9100

[Install]
WantedBy=multi-user.target
```

Here is a breakdown of the above:

**[Unit] Section**

- **Description**: Provides a brief description of the service. Here, "Node Exporter" indicates that this service is for running the Node Exporter.
- **Documentation**: Points to the online documentation. This is useful for someone who needs more information about the service.
- **Wants**: This line indicates that this service wants the `network-online.target` to be active, but it doesn't strictly require it. This means that the service prefers the network to be online but will start even if it's not.
- **After**: Ensures that this service is started after the `network-online.target`. This is useful for making sure the network is available for the Node Exporter to function correctly.

**[Service] Section**

- **User and Group**: Specify under which user and group the service will run. This is for security purposes, ensuring that the service has only the permissions it needs.
- **Type=simple**: This is the simplest and most common service type. It tells systemd to consider the service up as soon as the process is running.
- **Restart=on-failure**: Instructs systemd to restart the service if it fails (i.e., exits with a non-zero status code).
- **ExecStart**: Specifies the command to start the service. Here, it's starting the Node Exporter binary (`/etc/node_exporter/node_exporter-1.7.0.linux-amd64/node_exporter`) and setting a flag to listen on port 9100 for web traffic (`--web.listen-address=:9100`).

**[Install] Section**

- **WantedBy=multi-user.target**: This directive tells systemd to link this service file when the system is in a multi-user state (which is a common run level for fully operational, multi-user, networked systems). Essentially, it means that this service should be started by default in a regular, multi-user environment.

Once the file has been saved change the permissions on in `chmod 664 /usr/lib/systemd/system/node_exporter.service`

After changing the permissions you may need to create the `node_exporter` user and add it to the group so let us do that.

```bash
useradd -r -M -s /bin/false node_exporter
groupadd node_exporter
usermod -a -G node_exporter node_exporter
```

## Reload systemd and Start Node Exporter

Now reload the `systemd` service to register the prometheus service and start the prometheus service.

```bash
systemctl daemon-reload
systemctl start node_exporter
```

Check the node exporter service status using the following command.

```bash
systemctl status node_exporter

root@pve2:~# systemctl status node_exporter
● node_exporter.service - Node Exporter
     Loaded: loaded (/lib/systemd/system/node_exporter.service; disabled; preset: enabled)
     Active: active (running) since Tue 2024-02-20 12:18:18 GMT; 6s ago
       Docs: https://prometheus.io/docs/guides/node-exporter/
   Main PID: 1031633 (node_exporter)
      Tasks: 5 (limit: 38268)
     Memory: 2.5M
        CPU: 7ms
     CGroup: /system.slice/node_exporter.service
             └─1031633 /etc/node_exporter/node_exporter-1.7.0.linux-amd64/node_exporter --web.listen-add>
```

Configure node_exporter to start at boot

```bash
systemctl enable node_exporter.service
```

## Verify Node Exporter is Running

Verify the exporter is running by visiting the `/metrics` endpoint on the node on port `9100`. You should be able to see something similar to the following:

```
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
go_gc_duration_seconds{quantile="0.75"} 0
go_gc_duration_seconds{quantile="1"} 0
go_gc_duration_seconds_sum 0
go_gc_duration_seconds_count 0
# HELP go_goroutines Number of goroutines that currently exist.
...
```

## Clean up the install file

`cd` back to your `$HOME` directory or where you downloaded the `.tar.gz` file and run the following:

```bash
cd

rm -rf node_exporter-1.7.0.linux-amd64.tar.gz
```

# Configuring your Prometheus instances

The Prometheus instance needs to be properly configured to access Node Exporter metrics. The following `prometheus.yml` example configuration file will tell the Prometheus instance to scrape, and how frequently, from the Node Exporter via `localhost:9100`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
      - targets: ['localhost:9100'] # IP of the node you installed node_exporter on.
```
