# Frigate Network Video Recorder

This setup uses the [Amcrest IP5M-T1179EW-AI-V3 Camera](https://www.amazon.com/gp/product/B083G9KT4C?ie=UTF8&th=1&linkCode=sl1&tag=frigate0d-20&linkId=7462400a6ff3a326d7e0d0c7f6b93504&language=en_US&ref_=as_li_ss_tl).  

This setup also makes use of the Coral TPU for object detection resource offloading.

Frigate will run on a Docker container on this VM. Thus, this setup will include Docker, Docker Compose, and Portainer for easy GUI management.  

## Resources
### Initial Setup:
* [Jim's Garage - Frigate CCTV Install & Setup](https://www.youtube.com/watch?v=iO584g5c0DY&t=1220s)
* [Frigate in Proxmox LXC - Unprivileged with Intel iGPU (11th gen), USB Coral and Network share](https://github.com/blakeblackshear/frigate/discussions/5773)
* [Frigate Official Wiki - Getting Started](https://docs.frigate.video/guides/getting_started)
* [Reolink and Configuring Substreams for Record, Detect](https://www.reddit.com/r/frigate_nvr/comments/1g5r8o1/help_with_configuring_reolink_poe_doorbell_with/)

### Device Passthroughs and Configurations:
* [Coral TPU Driver Install for Linux](https://coral.ai/docs/m2/get-started/#requirements)
* [/dev/apex_0 Not Found](https://github.com/google-coral/edgetpu/issues/407)
* [Linux Virtual Machine iGPU Passthrough Configuration](https://3os.org/infrastructure/proxmox/gpu-passthrough/igpu-passthrough-to-vm/#linux-virtual-machine-igpu-passthrough-configuration)
* [Reddit - Frigate iGPU statistics](https://www.reddit.com/r/frigate_nvr/comments/1byj9pp/frigate_in_docker_in_unprivileged_lxc_has_no_igpu/)


## VM and Hardware Setup    

|               |               |
| ------------- | ------------- |
| Machine       | q35  |
| BIOS          | OVMF (UEFI) |
| Disk Size     | 20 GiB  |
| CPU Cores     | 4  |  
| CPU Type      | Host  |  
| Memory        | 4096 MiB  |
| Network       | Default  |  


## Coral TPU Setup  

Once the Coral TPU is physically installed in your Proxmox VE host server, you should see it as an available PCIe device for passthrough in the Hardware Options for your virtual machine.  

![image](https://github.com/user-attachments/assets/d2cb3898-c5ab-4c66-bb4a-9b0310ddd20a)



## Docker and Portainer Setup

From the [Docker Official Docs](https://docs.docker.com/engine/install/debian/), get Docker set up on the Linux machine with the instructed commands.  

Once Docker is successfully installed and the hello-world container is spun up to test, from the [Portainer Official Docs](https://docs.portainer.io/start/install-ce/server/docker/linux) you can proceed with setting up Portainer.  

Finally, to spin up a Frigate container, use the [attached docker-compose.yml file](docker-compose.yml), either in Portainer or in the command line, pull the image and start the app. Make sure you edit and review the configurations in the docker-compose.yml to match your system's requirements and desired directory locations for the volumes.  




