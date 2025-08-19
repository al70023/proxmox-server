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
# Allow only requests that start with /webhook/ OR /webhook-test OR oauth
if ($request_uri !~ ^/(webhook/|webhook-test|rest/oauth2-credential/callback)) {
  return 444;
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


## Update Troubleshooting

Sometimes, running "udpate" on the proxmox helper script will fail. Here is an example of the error given:  

```
3528 silly placeDep node_modules/n8n/node_modules/shelljs minimatch@3.1.2 OK for: glob@7.2.3 want: ^3.1.1 3529 silly fetch manifest brace-expansion@^1.1.7 3530 silly packumentCache full:https://registry.npmjs.org/brace-expansion cache-miss 3531 http fetch GET 200 https://registry.npmjs.org/brace-expansion 58ms (cache revalidated) 3532 silly packumentCache full:https://registry.npmjs.org/brace-expansion set size:46250 disposed:false 3533 silly placeDep node_modules/n8n brace-expansion@1.1.12 OK for: minimatch@3.1.2 want: ^1.1.7 3534 silly fetch manifest brace-expansion@^2.0.1 3535 silly packumentCache full:https://registry.npmjs.org/brace-expansion cache-hit 3536 silly placeDep node_modules/n8n/node_modules/minimatch brace-expansion@2.0.2 OK for: minimatch@9.0.5 want: ^2.0.1 3537 silly placeDep node_modules/n8n/node_modules/teeny-request @tootallnate/once@2.0.0 OK for: http-proxy-agent@5.0.0 want: 2 3538 silly placeDep node_modules/n8n/node_modules/teeny-request agent-base@6.0.2 OK for: http-proxy-agent@5.0.0 want: 6 3539 silly placeDep node_modules/n8n/node_modules/wrap-ansi-cjs color-convert@2.0.1 OK for: ansi-styles@4.3.0 want: ^2.0.1 3540 silly fetch manifest color-name@~1.1.4 3541 silly packumentCache full:https://registry.npmjs.org/color-name cache-miss 3542 http fetch GET 200 https://registry.npmjs.org/color-name 50ms (cache revalidated) 3543 silly packumentCache full:https://registry.npmjs.org/color-name set size:16325 disposed:false 3544 silly placeDep node_modules/n8n/node_modules/wrap-ansi-cjs color-name@1.1.4 OK for: color-convert@2.0.1 want: ~1.1.4 3545 silly placeDep node_modules/n8n/node_modules/wrap-ansi color-convert@2.0.1 OK for: ansi-styles@4.3.0 want: ^2.0.1 3546 silly placeDep node_modules/n8n/node_modules/wrap-ansi color-name@1.1.4 OK for: color-convert@2.0.1 want: ~1.1.4 3547 silly placeDep node_modules/n8n/node_modules/yamljs sprintf-js@1.0.3 OK for: argparse@1.0.10 want: ~1.0.2 3548 silly placeDep node_modules/n8n/node_modules/yamljs minimatch@3.1.2 OK for: glob@7.2.3 want: ^3.1.1 3549 silly placeDep node_modules/n8n/node_modules/@n8n/n8n-nodes-langchain agent-base@6.0.2 OK for: https-proxy-agent@5.0.1 want: 6 3550 silly reify mark retired [ '/usr/lib/node_modules/n8n', '/usr/bin/n8n' ] 3551 silly reify moves { 3551 silly reify '/usr/lib/node_modules/n8n': '/usr/lib/node_modules/.n8n-seyU16Gs', 3551 silly reify '/usr/bin/n8n': '/usr/bin/.n8n-i29c3Ryl' 3551 silly reify } 3552 verbose stack Error: ENOTEMPTY: directory not empty, rename '/usr/lib/node_modules/n8n' -> '/usr/lib/node_modules/.n8n-seyU16Gs' 3552 verbose stack at async Object.rename (node:internal/fs/promises:784:10) 3552 verbose stack at async moveFile (/usr/lib/node_modules/npm/node_modules/@npmcli/fs/lib/move-file.js:30:5) 3552 verbose stack at async Promise.allSettled (index 0) 3552 verbose stack at async [reifyPackages] (/usr/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:325:11) 3552 verbose stack at async Arborist.reify (/usr/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:142:5) 3552 verbose stack at async Update.exec (/usr/lib/node_modules/npm/lib/commands/update.js:63:5) 3552 verbose stack at async Npm.exec (/usr/lib/node_modules/npm/lib/npm.js:207:9) 3552 verbose stack at async module.exports (/usr/lib/node_modules/npm/lib/cli/entry.js:74:5) 3553 error code ENOTEMPTY 3554 error syscall rename 3555 error path /usr/lib/node_modules/n8n 3556 error dest /usr/lib/node_modules/.n8n-seyU16Gs 3557 error errno -39 3558 error ENOTEMPTY: directory not empty, rename '/usr/lib/node_modules/n8n' -> '/usr/lib/node_modules/.n8n-seyU16Gs' 3559 silly unfinished npm timer reify 1753497974166 3560 silly unfinished npm timer reify:retireShallow 1753497988105 3561 verbose cwd /root 3562 verbose os Linux 6.8.12-11-pve 3563 verbose node v22.17.0 3564 verbose npm v10.9.2 3565 verbose exit -39 3566 verbose code -39
```  
  
From ChatGPT:  
You're hitting a common error during your n8n update:
ENOTEMPTY: directory not empty, rename '/usr/lib/node_modules/n8n' -> '/usr/lib/node_modules/.n8n-xxxxxx'

This is an npm error
 (ENOTEMPTY) that means npm tried to move the old n8n directory out of the way but couldn't because something inside /usr/lib/node_modules/n8n is still there or in use.  
   
To fix this:  
1. Stop n8n  
  
Make sure n8n isn't running (otherwise files may be locked).

```
pm2 stop n8n
# or, if you're using systemd:
systemctl stop n8n
```

2. Check for Node Processes  
  
Make sure no node processes are using /usr/lib/node_modules/n8n:  

```
lsof +D /usr/lib/node_modules/n8n
```  

3. Manually Remove the n8n Directory  

If you're sure n8n is stopped:  

```
rm -rf /usr/lib/node_modules/n8n
```
  
This deletes the old directory that npm couldn't move.  
(Careful: this is permanent! If you're worried, back it up first.)  

4. Retry the Update

If this was successful, then you can safely restart n8n:  

```
# If you use PM2 (most common for manual installs):
pm2 start n8n
pm2 save

# If you use systemd (service):
systemctl start n8n
systemctl status n8n
```

If you get this error when getting the status of n8n:
```
× n8n.service - n8n
Loaded: loaded (/etc/systemd/system/n8n.service; enabled; preset: enabled)
Active: failed (Result: exit-code) since Fri 2025-07-25 22:01:09 CDT; 2s ago
Duration: 1ms Process: 309 ExecStart=n8n start (code=exited, status=203/EXEC)
Main PID: 309 (code=exited, status=203/EXEC)
CPU: 537us

Jul 25 22:01:09 n8n (n8n)[309]: n8n.service: Failed at step EXEC spawning n8n: No such file or directory
Jul 25 22:01:09 n8n systemd[1]: Started n8n.service - n8n.
Jul 25 22:01:09 n8n systemd[1]: n8n.service: Main process exited, code=exited, status=203/EXEC
Jul 25 22:01:09 n8n systemd[1]: n8n.service: Failed with result 'exit-code'.
```

It means that n8n is not in your system’s PATH. Confirm this by running `which n8n`, which should returns nothing because n8n cannot be found.  

To fix this: 

1. (Re)Install n8n Globally  

This will make n8n available globally:  

```
npm install -g n8n
```  

This should create `/usr/local/bin/n8n` (verify after with `which n8n`).  

2. Check Install and PATH
```
which n8n
# Should output: /usr/local/bin/n8n
n8n --version
# Should output the installed n8n version
```  

3. Update Your systemd Service (if needed)  

If `/usr/local/bin/n8n` exists, edit `/etc/systemd/system/n8n.service` to have:  

```
ExecStart=/usr/local/bin/n8n start
```  

4. Reload and Start Service
   
```
systemctl daemon-reload
systemctl start n8n
systemctl status n8n
```
