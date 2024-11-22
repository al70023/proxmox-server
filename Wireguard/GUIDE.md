# Wireguard VPN Tunnel

This setup requires using a Dynamic DNS service, to have a public address for your home network to point the VPN to. This will also require port forwarding 51820 on your home network router to the host IP address, so requests can reach your server running Wireguard.  


## Resources:
* [Proxmox Helper Scripts - Wireguard LXC](https://tteck.github.io/Proxmox/#wireguard-lxc)
* [Barmine Tech - Wireguard LXC Container Setup](https://www.youtube.com/watch?v=mXpoGu0xyGA&t=352s)
* [Barmine Tech - Updated Wireguard LXC Container](https://www.youtube.com/watch?v=hF9QQiqYRDI)
* [The $0 Home Server](https://youtu.be/IuRWqzfX1ik?si=pfoNZxvBvQwZNPMw&t=508)


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

*Note: These settings are automatically configured using the helper script install.  

|               |               |
| ------------- | ------------- |
| Disk Size     | 4 GiB  |
| CPU Cores     | 1  |  
| Memory        | 512 MiB   |
| Swap          | 512 MiB   |  
| IPv4          | 192.168.1.3/24 (static)  |
| DNS           | Host   |   


## WGDashboard Configuration:

Upon first login, the web GUI will take `admin` as the username and password. You can change this after by creating an account.  

Once logged in, go to Settings, and ensure DNS is set to your local DNS for both IPv4 and v6. Then change the Peer Remote Endpoint to the Duck DNS domain name pointing to your public IP.  

To create a new tunnel, go Home and create a new one using the plus button.  

Input the listen port that is the one forwarded in your router settings (51820).  

For IP Address/CIDR, use any local IP address that is not in the range of your machines on the network. If your machines are on `192.168.1.1` and up, choose something like `192.168.100.x/24` or `10.0.0.1/24`.  

Once saved, create a new Peer with a `/32` in the range specified for the tunnel.  

If full tunnel connection, Endpoint Allowed IPs should be `0.0.0.0/0, ::/0`.  

Otherwise, if split tunnel to home network, enter your IPv4 and IPv6 addresses, such as `192.168.1.0/24, xxxx:xxxx:xxxx:xxxx::/64`.  

Create the peer, and connect a device using the configuration.  



## Test Wireguard VPN's Functionality:
* Set up a client, such as a smartphone or PC, with the Wireguard configuration.
* Make sure you are not connected to the home network.
* Browse the internet, and try accessing an internal service, such as one on the `192.168` network. You should be able to access it, and see the requests in AdGuard Home, as the VPN uses the local DNS.
* Also, set up a split tunnel on your phone with On-Demand, excluding your home network, and enabling on wifi and data, to always be connected to the local DNS.
  * For split tunnels, specify allowed IPs to be only the home network subnet `192.168.10.0/24` and optionally, the IPv6 subnet, found on the router settings ending in `::/64`.
