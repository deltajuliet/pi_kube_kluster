# pi_kube_kluster
Initial installation steps:

## Imaging
1. Download [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/) image.
2. Download and install [balenaEtcher](https://www.balena.io/etcher/)
3. Image SD card with Raspbian Lite

## Initial headless configuration
1. Enable SSH (for Buster) `touch /Volumes/boot/ssh`
2. Configure WIFI `vim /Volumes/boot/wpa_supplicant.conf`
Then add the following:
```
country=US # Your 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
```
3. Unmount SD card `sudo diskutil unmount /Volumes/boot`
4. Insert SD card in Pi and power up
5. SSH using pi:raspberry
6. Change default password
7. Install VIM `sudo apt-get install vim`
8. Change the hostname `sudo vim /etc/hostname`
9. Reboot
10. Setup DHCP reservation on router
11. Add host to local /etc/hosts file `sudo vim /etc/hosts` and change /etc/hosts `sudo vim /etc/hosts`:
```
192.168.1.70    pi-kube-master
192.168.1.71    pi-kube-node01
192.168.1.72    pi-kube-node02
192.168.1.73    pi-kube-node03
```
12. Copy local SSH key to pi `ssh-copy-id [PI_HOSTNAME_OR_IP]`

## Setup k3s master node
1. Generate an ssh key `ssh-keygen`
2. Push key to other node `ssh-copy-id pi@[NODE_HOSTNAME]`
3. Setup CPU for containers `vim ` and then add `cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory`

## References
-[k3s Github Page](https://github.com/rancher/k3s)
-[Pi Dramble](https://github.com/geerlingguy/raspberry-pi-dramble)
-[https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)
