# Wireguard VPN Tunnel

This setup requires using a Dynamic DNS service, to have a public address for your home network to point the VPN to. This will also require port forwarding 51820 on your home network router to the host IP address, so requests can reach your server running Wireguard.  


## Resources:
* [Proxmox Helper Scripts - Wireguard LXC](https://tteck.github.io/Proxmox/#wireguard-lxc)
* [Barmine Tech - Wireguard LXC Container Setup](https://www.youtube.com/watch?v=mXpoGu0xyGA&t=352s)
* [Barmine Tech - Updated Wireguard LXC Container](https://www.youtube.com/watch?v=hF9QQiqYRDI)


## Duck DNS Setup:

A Dynamic DNS is required if you need to find your home network and you don’t have a static IP address. Most ISPs don’t provide a static IP address or change your IP address periodically. If you don’t know if you have a static IP address, you can set up a DDNS regardless.

A DDNS is a service that gives you a hostname (a web address) that always points to your home network’s public IP address. There are many DDNS services, but this setup will use Duck DNS, which is free, permanent, and simple to use.  

Go to [duckdns.org](https://www.duckdns.org/) and sign in using your preferred method. Then, create a domain with any name that you like, as long as it isn’t already taken.  

![image](https://github.com/user-attachments/assets/66a0f908-1178-4448-a2d9-64ec0b185d57)  

Then, click on install, go the the Linux Cron section, and follow the instructions you see on the page.  

![image](https://github.com/user-attachments/assets/ee91c4fc-5315-4aff-92fd-70f34e924191)  


## Port Forwarding:

In order for Wireguard to work, you will need to forward port 51820 in your router’s web interface.  

Make sure to select the IP of your server as the LAN IP Address, 51820 as the port on both WAN and LAN and use UDP as the protocol.  

   
## Container Setup:  



**To test Wireguard VPN's functionality:** 
* Set up a client, such as a smartphone or PC, with the Wireguard configuration.
* Make sure you are not connected to the home network.
* Browse the internet, and try accessing an internal service, such as one on the `192.168` network. You should be able to access it, and see the requests in AdGuard Home, as the VPN uses the local DNS.
* Also, set up a split tunnel on your phone with On-Demand, excluding your home network, and enabling on wifi and data, to always be connected to the local DNS.
  * For split tunnels, specify allowed IPs to be only the home network subnet `192.168.10.0/24` and optionally, the IPv6 subnet, found on the router settings ending in `::/64`.
