# N8n


## Setting up Webhooks for Public External Internet Access

This setup will show how to open up port forwarding to hit webhooks from external services.  

First, set environment variables to a public DNS address that points to your public IP. Environmment variables can be found at `/etc/systemd/system/n8n.service`  
Example:  
```
[Unit]
Description=n8n

[Service]
Type=simple
Environment="N8N_SECURE_COOKIE=false"
Environment="WEBHOOK_URL=https://n8n-public.thefakeness23.duckdns.org/"
Environment="N8N_EDITOR_BASE_URL=https://n8n-public.thefakeness23.duckdns.org/"
ExecStart=n8n start
[Install]
WantedBy=multi-user.target
```

Then, Port Forward HTTP, HTTPS on OPNsense Router to Nginx Proxy Manager (in this case found at `192.168.1.3`):  
  
![image](https://github.com/user-attachments/assets/e03d764d-064c-412d-a1b8-286b361a2157)  

![image](https://github.com/user-attachments/assets/b316b123-5598-4fce-a2a3-6160bc21c4a8)  

### In Nginx Proxy Manager:  

![image](https://github.com/user-attachments/assets/af72947f-a6ab-4577-abc1-43119ebdaa66)  

For the Proxy Host, add this section in the Advanced tab for added security: only open up webhooks to public internet access, public anything else like Web GUI access:  
  
![image](https://github.com/user-attachments/assets/344de078-4b5a-4f98-8831-1a3c67847b9a)  


```
# Allow only requests that start with /webhook/ OR /webhook-test
if ($request_uri !~ ^/(webhook/|webhook-test)) {
  return 403;
}
```
  
Create a second Nginx Proxy Host with the base domain and have it block access, so that only the n8n subdomain meant for webhooks will resolve:  
  
![image](https://github.com/user-attachments/assets/39c391b5-eea2-482a-90da-82e6d6faf6a0)  


```
# Custom Nginx Configuration:
return 444;
```  

Remember to add a new DNS rewrite in your DNS resolver for speed internally: 

![image](https://github.com/user-attachments/assets/7c91a3b0-659e-48d1-be54-1e7536c16479)  

### --

For added security, add a GeoIP rule to limit hitting the public address only from USA. Go to Firewall -> Aliases -> GeoIP settings and follow [this guide](https://docs.opnsense.org/manual/how-tos/maxmind_geo_ip.html).  

Add a new Alias in GeoIP like so:  

![image](https://github.com/user-attachments/assets/277b83af-1a29-40b5-a01b-b3301fb5c235)  

And then make sure to add it as a source in your NAT port forward rules:  

![image](https://github.com/user-attachments/assets/41ec5a8b-1ec8-4e24-9ed4-b6f18b4552c4)  
  
### --

Optionally change OPNsense GUI port from 443 to something else like 1443 to avoid conflicts when hitting 192.168.1.1  

![image](https://github.com/user-attachments/assets/a57d2fdb-6486-4efc-ab45-bb00bc6b6e17)  



