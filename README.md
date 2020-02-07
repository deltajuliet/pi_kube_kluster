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
7. Install VIM ``
8. Change the hostname `sudo vim /etc/hostname`
9. Reboot
10. Setup DHCP reservation on router
11. Add host to local /etc/hosts file `vim /etc/hosts`

## Prep for k3s
1. Setup CPU for containers `vi``cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory`


## References
[k3s Github Page](https://github.com/rancher/k3s)
[Pi Dramble](https://github.com/geerlingguy/raspberry-pi-dramble)
[https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)
