# N8n

Environmment variables can be found at `/etc/systemd/system/n8n.service`
Examples:

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

Open up port forwarding to hit webhooks from external services.
  
Set environment variables to a public DNS address that points to your public IP.

Port forward HTTP, HTTPS on router to Nginx Proxy Manager:
![image](https://github.com/user-attachments/assets/e03d764d-064c-412d-a1b8-286b361a2157)  


![image](https://github.com/user-attachments/assets/b316b123-5598-4fce-a2a3-6160bc21c4a8)  


Optionally change OPNsense GUI port from 443 to something else like 1443 to avoid conflicts when hitting 192.168.1.1  

![image](https://github.com/user-attachments/assets/a57d2fdb-6486-4efc-ab45-bb00bc6b6e17)  


And in Nginx Proxy Manager:

![image](https://github.com/user-attachments/assets/af72947f-a6ab-4577-abc1-43119ebdaa66)  

For the Proxy Host, add this section in the Advanced tab for added security: only open up webhooks to public internet access, public anything else like Web GUI access:  
![image](https://github.com/user-attachments/assets/c7c1c812-a4cb-49c3-9a4b-b43981988c4c)  

```
# Return 403 for all requests not starting with /webhook/
if ($request_uri !~ ^/webhook/) {
  return 403;
}
```
  


Remember to add a new DNS rewrite in your DNS resolver for speed internally: 

![image](https://github.com/user-attachments/assets/7c91a3b0-659e-48d1-be54-1e7536c16479)  



