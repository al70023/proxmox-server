# OpenMediaVault Network Attached Storage

This setup uses a physical hard disk passed through to the OMV VM as shown in the video and wiki guides. This will pass the entire disk as a SCSI device.

## Resources
* [Installing Open Media Vault on Proxmox made EASY](https://www.youtube.com/watch?v=Bce7VT3kJ4g)
* [Proxmox Wiki - Passthriugh Physical Disk to Virtual Machine](https://pve.proxmox.com/wiki/Passthrough_Physical_Disk_to_Virtual_Machine_(VM))


## VM and Hardware Setup    

|               |               |
| ------------- | ------------- |
| System        | Default  |
| Disk Size     | 16 GiB  |
| CPU Cores     | 2  |  
| CPU Type      | Host  |  
| Memory        | 4096 MiB  |
| Network       | Default  |


## Web GUI Setup

Once the VM is installed through the ISO, navigate to the VM's IP address to access the Web GUI.  

The initial login will `user: admin` and `password: openmediavault`.  

Now, make sure to change the default password, install updates, and reboot the VM to apply changes.  


### RAID and File Shares

1. To create RAID configurations with multiple disks, install the "openmediavault-md" plugin.  

2. After the install, you should see a new tab under the 'Storage' section called 'Multiple Devices'. Here, you can link multiple drives.  

3. If using a single drive, or once RAID has been set up, you can go to the 'File Systems' tab, and create a new EXT4.  

4. Once the Filesystem is created, mount it.  

5. Now, navigate to the 'Shared Folders' tab and create a new shared folder for the filesystem.  

6. Under the 'Services' section, go to the 'NFS' tab, and select the created shared folder, and create a share for it over the network.
   * For `Clients`, specify the subnet of clients that will mount the file system, such as `192.168.1.0/24`.
   * Ensure proper permissions (Read, Read/Write)
  
7. Finally, under the NFS service tab, go to Settings, and ensure the service is enabled.


