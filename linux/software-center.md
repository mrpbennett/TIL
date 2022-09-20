# Getting Software center back for installing applications

I have been trying out Ubuntu and I was unable to install applications till I
came across the below

The Ubuntu Software Center (software-center) has been upgraded to GNOME Software
(gnome-software) in Ubuntu 16.04 and later. The software app has also been
renamed to Software in Ubuntu 20.04 and later.

If the Software app doesn't open run the following commands:

```bash
sudo apt clean # clean list of cached packages so Ubuntu Software can read them
sudo apt update && sudo apt upgrade
sudo apt autoremove gnome-software && sudo apt install gnome-software
```
