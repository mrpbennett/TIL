# Change Hostname on Ubuntu

Another way to permanently change the hostname is by editing two configuration files:

- `/etc/hostname`
- `/etc/hosts`
-

The changes take effect immediately after system reboot.

## Step 1: Open /etc/hostname and Change the Hostname

Edit the file with a text editor of your choice. In this example, we will be using the Vim editor:

```bash
sudo nano /etc/hostname
```

The `/etc/hostname` file contains only the current hostname. Replace it with your new choice.

Save the file and exit.

## Step 2: Open /etc/hosts and Change the Hostname

Now edit the /etc/hosts file in the same way.

```bash
sudo vi /etc/hosts
```

The file `/etc/hosts` maps hostnames to IP addresses. Look for the hostname you wish to change and simply replace it with your new choice.

Save the edits and exit.

## Step 3: Reboot the System

Reboot your computer to apply the changes:

```bash
sudo systemctl reboot
```
