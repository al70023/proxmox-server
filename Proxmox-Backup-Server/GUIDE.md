# Proxmox Backup Server

This setup uses a physical hard disk passed to the OpenMediaVault VM as the mounted target drive for backups.

## Resources
* [Jim's Garage - Proxmox Backup Server](https://www.youtube.com/watch?v=84QZc5cnKZc&t=875s)  

## VM and Hardware Setup    

|               |               |
| ------------- | ------------- |
| System        | Default  |
| Disk Size     | 20 GiB  |
| CPU Cores     | 2  |  
| CPU Type      | Host  |  
| Memory        | 4096 MiB  |
| Network       | Default  |


## Web GUI Setup

Once the VM is installed and rebooted, navigate to the 'Adminstration' tab.  

Configure 'No Subscription' environment and run updates.  

Then, navigate to 'Certificates' tab and set a self-signed certificate for the web GUI.  

Use the OpenMediaVault guide to mount the fileshare for backup storage.  



