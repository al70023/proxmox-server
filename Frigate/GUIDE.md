# Frigate Network Video Recorder

This setup uses the [Amcrest IP5M-T1179EW-AI-V3 Camera](https://www.amazon.com/gp/product/B083G9KT4C?ie=UTF8&th=1&linkCode=sl1&tag=frigate0d-20&linkId=7462400a6ff3a326d7e0d0c7f6b93504&language=en_US&ref_=as_li_ss_tl)

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
