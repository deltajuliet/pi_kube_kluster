# pi_kube_kluster
Initial installation steps:

## Imaging
1. Download (Raspbian Lite)[https://www.raspberrypi.org/downloads/raspbian/] image.
2. Download and install [balenaEtcher][https://www.balena.io/etcher/]
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
7. Change the hostname `sudo vi /etc/hostname`
8. Reboot
9. Setup DHCP reservation on router

## Prep for K3S
1. Setup CPU for 


## References
https://github.com/rancher/k3s
https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/
