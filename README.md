# Overview

High level network layout of my homelab. Updated as of March 10, 2025.

![Home Network Infrastructure Diagram](Diagrams/3-10-25.png)


# Getting Started
* If using laptop, make sure to disable sleep on lid close before any install.
* Before installation, set Virtualization and IOMMU (PCIE passthrough) on in BIOS settings
* Backup any data that may be lost.  
* Run benchmarks for CPU, check NIC max throughput, and check network speeds.

#### Resources:
* [Techno Tim - First 11 Things on Proxmox](https://www.youtube.com/watch?v=GoZaMgEgrHw&t=91s&pp=ygUIcHJveG1vbXg%3D)
* [Christian Lempa - Proxmox Must Haves](https://www.youtube.com/watch?v=VAJWUZ3sTSI&t=1010s&pp=ygUIcHJveG1vbXg%3D)
* [Jim's Garage - Proxmox Installation](https://www.youtube.com/watch?v=jNEjKdtMrZI&t=1014s&pp=ygUUcHJveG1vbXggamltcyBnYXJhZ2U%3D)
* [Jim's Garage - Proxmox LXC](https://www.youtube.com/watch?v=xKhWRMj5Nrc&t=342s&pp=ygUUcHJveG1vbXggamltcyBnYXJhZ2U%3D)
* [Changing Proxmox Host IP Address](https://www.servethehome.com/how-to-change-primary-proxmox-ve-ip-address/)
  

## Proxmox Virtual Environment Download

1. Download [Proxmox VE ISO Installer](https://www.proxmox.com/en/downloads)

2. Flash the ISO to a USB Stick using Balena Etcher
   * Make sure no important data is on the USB, it will be wiped
  
## Installation

Follow the prompts on screen to install Proxmox VE. 
* Set the hostname to something not `.local` or `.com` (good suggestion is `.lan`)
* Set the IP address to something available on your home network, and the correct internet gateway and DNS server

## First Login

To log in to the proxmox server, enter `root` as the username and your selected password. Once in, you should get a dialogue saying "No valid subscription." This will be addressed next.      


### 1. Configure Updates

Navigate to your proxmox host, and then the `Repositories` tab under `Updates`.  

![image](https://github.com/user-attachments/assets/e2fed01e-5c4c-440a-a4f5-a2b4339922b5)  

Disable the two repositories with "enterprise" and Add a new reposiory with the "No-subscription" title. 

Click the `Updates` tab and hit "refresh".  

![image](https://github.com/user-attachments/assets/0176c3af-f439-4b16-9300-f92130ab6c54)  

Finally, hit the "Upgrade" button.  


### 2. Set Trusted TLS Certificate

This will require a valid domain name through a provider like DuckDNS or Cloudflare, linked to the internal IP address of the proxmox host.  

Under your `Datacenter`, go to the `ACME` tab.  

![image](https://github.com/user-attachments/assets/e04a0d42-58f0-4b3e-a13f-fe0b4f0baa4e)  

Click on Add under "Challenge Plugins", input any name, such as "duckdns", select the provider, and input the token, in this case formatted as:  

  ```
  DuckDNS_Token=MYTOKEN
  ```

Next, click on Add under "Accounts", input any name, such as reusing "duckdns", enter your email, and choose the first option from the dropdown menu "Let's Encrypt V2".  

Under your node (the host machine), go to the `Certificates` tab, click Add, and choose "DNS" as challenge type, select your created "duckdns" plugin, and input a subdomain name in the domain of your duckdns domain.  

![image](https://github.com/user-attachments/assets/20dcba64-2b46-42ee-a893-ae452a249f36)  

Choose your duckdns account for "Using account", then click "Order Certificates Now".  

After reloading and navigating to [YOUR-SELECTED-DOMAIN]:8006 you should see your proxmomx web GUI, trusted by the browser now.  


### 3. Reduce disk writes to preserve SSD lifespan

* [Reddit - Proxmox folder2ram versus log2ram](https://www.reddit.com/r/Proxmox/comments/129dxw7/proxmox_high_disk_writes/)
* [Reddit - Proxmox folder2ram and journald configs](https://www.reddit.com/r/Proxmox/comments/12gftf7/reduce_wear_on_ssds/)
* [Proxmox Forum - Reducing rrdcached writes](https://forum.proxmox.com/threads/reducing-rrdcached-writes.64473/) 


# LXC Containers

## 1. AdGuard Home DNS  

This will be a local DNS Server with blocklists and an upstream resolver.   

All setup can be found [here](AdGuard-Home/GUIDE.md).  


## 2. Nginx Proxy Manager  

This will serve as local DNS rewrites for docker services running on the server.   

Users will no longer use IP-ADDRESS:PORT to navigate to a service on their browser, and instead use the hostname assigned.  

This will also use Let's Encrypt SSL certificates, for HTTPS encryption on the browser when using these services.  

All setup can found [here](Nginx-Proxy-Manager/GUIDE.md).  


## 3. Homepage Dashboard  

This dashboard will be an all-encompassing holistic view of the entire homelab setup.   

All setup can found [here](Homepage/GUIDE.md).  


## 4. (Not In Use Currently) Wireguard VPN Tunnel  

This will allow securely tunneling into the host machine from any external client not on the same home network.  

All setup can be found [here](Wireguard/GUIDE.md).   


# Virtual Machines

## 1. OPNsense Firewall & Router  

This will be a full-scale home network router and firewall.  

All setup can be found [here](OPNsense/GUIDE.md).  


## 2. Frigate Network Video Recorder

This will be a completely self-hosted home CCTV surveillance/security system.  

This is a Debian 12 Linux Machine running Docker, with Frigate as a Docker container, as well as [Ntfy](Ntfy/GUIDE.md) and [Frigate-Notify](Frigate-Notify/GUIDE.md) for notifications.  

All setup can be found [here](Frigate/GUIDE.md).  


## 3. OpenMediaVault Network Attached Storage

This will be a virtualized self-hosted NAS solution, passing in a hard drive as the device to store on.  

All setup can be found [here](OpenMediaVault/GUIDE.md).  


## 4. Proxmox Backup Server

This will be a virtualized instance of Proxmox Backup Server, as a VM on the Proxmox host. This setup will mount the OpenMediaVault NAS drive as the device to store on.  

All setup can be found [here](Proxmox-Backup-Server/GUIDE.md).  



# Miscellaneous

* [How to add disks to a Proxmox server](https://www.youtube.com/watch?v=tKD-dgSKBxU&t=181s)
* [How to add an additional hard disk to a VM in Proxmox VE](https://www.youtube.com/watch?v=vI8myYHQgnA)
