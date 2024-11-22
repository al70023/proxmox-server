# AdGuard Home DNS

This LXC container will be set up manually with a static IP address. 
  
Once the container is spun up, navigate to `x.x.x.x:3000` to log in, where `x.x.x.x` is the IP address of the LXC container.   

## Resources:
* [AdGuard Home Installataion Guide](https://github.com/AdguardTeam/AdGuardHome#getting-started)

   
## Container Setup:  

|               |               |
| ------------- | ------------- |
| Disk Size     | 8 GiB  |
| CPU Cores     | 2 (cpu limit = 2)  |  
| Memory        | 2048 MiB  |
| Swap          | 512 MiB   |  
| IPv4          | 192.168.1.2/24 (static)  |
| DNS           | Host   |   

  


**To test AdGuard Home's functionality:** 
* Point any client device's IPv4 and IPv6 DNS to the IP address chosen for the container.
* Navigate to some websites, and check the AdGuard Home Query log to see the requests.
* You should see those requests being reflected. AdGuard Home is now serving as the DNS resolver for your client device.
* Ensure web browser is not using its own DNS server settings (close and reopen browser after making changes)  

Any requests to AdGuard that come from the host, such as `dig` or VPN connections, will show up in the AdGuard query logs under the IP address of the macvlan interface.  

***Note:** 
* AdGuard Home will stop YouTube history from working properly on mobile. To fix this, add this line to Custom Filtering Rules:
    
  ```
  @@||s.youtube.com^$important
  ```

