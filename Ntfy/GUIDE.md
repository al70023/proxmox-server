# Ntfy Notification Server

This self-hosted application will run in a docker container to provide instant notifications for connected apps and services.  

## Resources:
* [Ntfy Official Docs](https://docs.ntfy.sh/install/)
* [Ntfy Docker Installation](https://docs.techdox.nz/ntfy/)
* [Network Chuck - Ntfy](https://www.youtube.com/watch?v=poDIT2ruQ9M&t=640s&pp=ygUEbnRmeQ%3D%3D)
* [Techdox - Ntfy self-host guide](https://www.youtube.com/watch?v=79wHc_jfrJE&t=22s&pp=ygUEbnRmeQ%3D%3D)

## Setup

To ensure timely receival of notifications, follow the setup in [server.yml](server.yml), as also explainhed in the Network Chuck video.  

For proper integration with Frigate-Notify, attachments must be enabled. To do this, follow the official docs, and include the line in server.yml to point to an attachments-cache-dir.  

