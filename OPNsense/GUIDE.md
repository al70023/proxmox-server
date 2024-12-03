# OPNsense Router and Firewall

## Resources:
### Prior Learning
* [Jim's Garage - Learn vLANs, Subnets, and NAT to Improve Your Network Security](https://www.youtube.com/watch?v=gk_kHgNhJVo)
### Install and Setup
* [Jim's Garage - How to Set Up OpnSense (Part 1)](https://www.youtube.com/watch?v=vJBoCgptF-0&t=10s&pp=ygUUamltcyBnYXJhZ2Ugb3Buc2Vuc2U%3D)
* [Jim's Garage - How to Configure OpnSense (Part 2)](https://www.youtube.com/watch?v=UI5tO1hP2q8&t=1170s)
* [Home Network Guy - Beginner's Guide to Set up a Full Network using OPNsense](https://www.youtube.com/watch?v=CXp0CgilMRA)
### Additional Configuration and Management
* [Configuring a Management VLAN](https://www.youtube.com/watch?v=9hJyWaQ2x28)
* [pfSense Baseline Guide (doc)](https://nguvu.org/pfsense/pfsense-baseline-setup/)
* [Home Network Guy - OPNsense Firewall Rule "Cheat Sheet"](https://homenetworkguy.com/how-to/firewall-rules-cheat-sheet/)
* [OPNsense Documentation - Firewall Rules Examples](https://www.zenarmor.com/docs/network-security-tutorials/how-to-configure-opnsense-firewall-rules#opnsense-firewall-rules-examples)

## To Do:

Register for ET telemetry rules and enable IPS. Configure IPS policy to categorize rules to drop anything malicious. Run it on all local ports instead of WAN if you don't explicitly allow incoming connections, so malicious traffic will be blocked on local network level, preventing potentially infected local device to spread its stuff locally.  

Use NAT rules to redirect every DNS queries to your local resolver. To Unbound on the OPNSense box for example. Same for NTP.  

Use the GeoIP module to build blacklist of countries you don't trust.  

Use DNSBL-s to block shady domains, DoH, dynamic IP hosts.  

Build your IP blacklists (using aliases) with lists like Firehol, and block them with a floating firewall rule.  

Disable the default LAN to any allow rule and whitelist things you need manually.  

Use crons to keep the lists up-to-date.  
