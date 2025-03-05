# Frigate-Notify

This lightweight docker container will act as a bridge between the Frigate NVR application and Ntfy service. It can also be configured to use other notification service apps, such as Gotify, Discord, Pushover, etc.  

## Resources:
* [Frigate-Notify Official Docs](https://frigate-notify.0x2142.com/latest/install/)

## Setup

Note that for integration with Ntfy, attachments are required to be enabled in Ntfy. This is because Frigate-Notify includes a screenshot of the Frigate alert in the notification, which is included as an attachment. For more info, see [this issue](https://github.com/0x2142/frigate-notify/issues/227) opened by me.  

Follow the official docs for step by step setup of the config.yml, or use [this config.yml file](config.yml) as reference.  
