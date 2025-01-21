# OpenMediaVault Network Attached Storage

This setup uses a physical hard disk passed through to the OMV VM as shown in the video and wiki guides. This will pass the entire disk as a SCSI device.

## Resources
* [Installing Open Media Vault on Proxmox made EASY](https://www.youtube.com/watch?v=Bce7VT3kJ4g)
* [Proxmox Wiki - Passthriugh Physical Disk to Virtual Machine](https://pve.proxmox.com/wiki/Passthrough_Physical_Disk_to_Virtual_Machine_(VM))
* [Linux NFS Server: How to Set Up Server and Client](https://bluexp.netapp.com/blog/azure-anf-blg-linux-nfs-server-how-to-set-up-server-and-client#H_H6)


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


## Mounting NFS Shares to Clients

1. Open a shell in the client Linux machine on which you wish to mount the NFS.
   
2. Create a local directory that will be used to mount the file share, using the following command:
   ```
   sudo mkdir /mnt/openmediavault
   ```

3. Mount the file share by running the mount command, as follows. There is no output if the command is successful.
   ```
   sudo mount -t nfs [NFS SERVER IP ADDRESS]:/[NFS FOLDER NAME] [YOUR/DESIRED/MOUNT/PATH]
   ```

   For example:
   ```
   sudo mount -t nfs 192.168.1.50:/myshareddir /mnt/openmediavault
   ```

4. To verify that the NFS share is mounted successfully, run `df -h`. The filesystem is now temporarily mounted in this machine session.

5. To permanently mount the file share, edit the following file with this command: `sudo nano /etc/fstab`.

6. Insert a line at the end of the file as following:
   ```
   [NFS SERVER IP ADDRESS]:/[NFS FOLDER NAME] [YOUR/DESIRED/MOUNT/PATH] nfs defaults 0 0
   ```

7. Run `systemctl daemon-reload` to update the new fstab version.  

8. Finally, mount the file share using the following commands. The next time the system starts, the folder will be mounted automatically.
   ```
   mount [YOUR/DESIRED/MOUNT/PATH]

   mount [NFS SERVER IP ADDRESS]:/[NFS FOLDER NAME]
   ```

You should now be able to reboot the machine and still see your mount permanently in the filesystem.   


## File Permissions

If you cannot write to the file share on the client, navigate to 'Shared Folders' under the 'Storage' section on the OMV web UI.  

Select the file share and click the Access Control List (ACL) button.  

Edit the configuration to apply permissions recursively, and change 'Others' to have Read/Write/Execute permissions.  
