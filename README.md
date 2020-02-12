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
2. Push key to other nodes `ssh-copy-id pi@[NODE_HOSTNAME]`
3. Setup CPU for containers `vim ` and then add `cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory` to the end of the first line in `/boot/cmdline.txt`
4. Reboot
5. Install k3s `curl -sfL https://get.k3s.io | sh -`
6. Validate the server is up and running `sudo systemctl status k3s`
7. Copy the server token `sudo cat /var/lib/rancher/k3s/server/node-token`

## Setup other nodes
1. Setup CPU for containers `vim ` and then add `cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory` to the end of the first line in `/boot/cmdline.txt`
2. Reboot
3. Setup environmental variables for k3s agent installation:
```
export K3S_URL="https://[MASTER_IP_ADDRESS]:6443"
export K3S_TOKEN="[MASTER_SERVER_TOKEN]"
```
4. Install k3s `curl -sfL https://get.k3s.io | sh -`

## Validate installation
1. List out nodes `sudo kubectl get node -o wide`

## Multi-interface quirks
1. Configure the eth0 interfaces with addresses referened above by using the `/etc/dhcpcd.conf` file to statically assign IP addresses based on fallback profiles.
2. Use the following install command for the k3s master node `curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --advertise-address=192.168.1.70 --bind-address=192.168.1.70" sh -`

## References
-[k3s Github Page](https://github.com/rancher/k3s)
-[Pi Dramble](https://github.com/geerlingguy/raspberry-pi-dramble)
-[https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)
