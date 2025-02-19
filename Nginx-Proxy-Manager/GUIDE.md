# Nginx Proxy Manager

This LXC container uses tteck's Proxmox Helper Script for setup.  
  
Once the container is spun up, navigate to `x.x.x.x:81` to log in, where `x.x.x.x` is the IP address of the container.   


## Resources:
  * [Proxmox VE Helper Scripts](https://tteck.github.io/Proxmox/)
  * [Self Hosting on Your Home Server - Cloudflare + Nginx Proxy Manager](https://www.youtube.com/watch?v=GarMdDTAZJo&t=567s)


## Duck DNS Setup:

This setup will use Duck DNS again, but pointing to an internal IP address. The domain name will be used to validate ownership and create SSL certificates that can be attached to proxy hosts, for encrypted connections to your services.      

Use existing subdomain that points to an internal network IP address (i.e. `192.168.x.x`).  

If you do not already have one, go to [duckdns.org](https://www.duckdns.org/) and sign in using your preferred method. Then, create a domain with any name that you like, as long as it isn’t already taken.  

Replace the IP address on this subdomain with the **address of your LXC container** on the home network, so that this domain can only be accessed internally within your home network, i.e. `192.168.10.x`.  

If you use an IP address other than the guest that is running NPM, the proxy redirects will not work, as first the connection must hit the "machine" that is runnig NPM before routing to the desired service.   


## Adding SSL Certificates:  

Once logged into the Nginx Proxy Manager web GUI, go to the "SSL Certificates" tab, select "Add SSL Certificate", and choose "Let's Encrypt" from the dropdown.  

In the **Domain Names** text box, enter two domains, separated by spaces:
1. Your Duck DNS subdomain in this format: `[YOUR-SUBDOMAIN].duckdns.org`
2. A wildcard subdomain for your Duck DNS subdomain in this format: `*.[YOUR-SUBDOMAIN].duckdns.org`  

Select "Use a DNS Challenge" and select Duck DNS from the list of providers. Paste your API token from your Duck DNS account by logging back in there, change the propogation seconds to 120, and then hit save.    

It should take a while and then give you back a success. If you receive errors, try again, it might take some time for your domain name to propogate.  

## Creating Proxy Hosts:  

Now, go to your dashboard, and select the "Proxy Hosts" tab.  

Select "Add Proxy Host".  

For each of the previously set up services, such as AdGuard Home, Vaultwarden, Wireguard, and Portainer, set up a subdomain to access it securely with SSL certificates.  

The format of this subdomain name should be `[SERVICE].[YOUR-SUBDOMAIN].duckdns.org`, where `[SERVICE]` can be anything you like to name the service, such as wireguard or wg for Wireguard, and `[YOUR-SUBDOMAIN]` is the one chosen for Duck DNS, which you also created SSL certificates for.  

Create a domain name, use the name of the container in **Forward Hostname/IP**, and the port of the web GUI for that service. Select the SSL certificate created for your subdomain, and attach it to the proxy host.   
 
