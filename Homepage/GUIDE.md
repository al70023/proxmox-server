# Homepage Dashboard

This setup makes use of the Proxmox Helper Script to deploy this service in an LXC container.  

## Resources:
* [Proxmox Helper Scripts - Homepage](https://tteck.github.io/Proxmox/#homepage-lxc)
* [Homepage Official Docs](https://gethomepage.dev/configs/)
* [Techno Tim - Meet Homepage](https://www.youtube.com/watch?v=mC3tjysJ01E)

## Configuration

Before beginning to edit the config files, create a .env file in the root directory of the homepage project. This .env will store API keys and login information for the various apps and services, to prevent hardcoding directly into config files.  

To insert locally saved photos within the config file, use the absolute path, and place the images in the `/homepage/public` folder.  
  
As of new version, must add `HOMEPAGE_ALLOWED_HOSTS= W.X.Y.X:12345,homepage.domain.org` to .env file.  
