---
layout: post
title: "Homelab Beginnings"
---

# The Beginnings

I have been wanting to set up some sort of sandbox environment to improve on my skills as I progress through my studies and career in IT. I have decided to go with a power efficient home lab enviroment to start off with. I will be using this blog as a way to document my learning as I tinker with different technologies and the state of my homelab. There are plans to expanding my homelab, but to what extent is currently unknown. 

## Hardware

All my research ended with me purchasing a powerful little box that is the Intel NUC. I have purchased the Intel NUC 12 Pro Wall Street Canyon (NUC12WSHi5) which includes a 2TB NVMe WD_BLACK SSD and two sticks of Kingston's 16GB DDR4 memory. The biggest thing I had in mind when purchasing a device for my virtualization solution was power efficiency. 

## Hypervisor

I decided to go the VMWare route as it is widely used in enterprise environments.  ESXi 7.0 Update 3 was installed onto the NUC however, I had to use the [Community Networking Driver for ESXi](https://flings.vmware.com/community-networking-driver-for-esxi). This is because the network interface card (Intel i225) is not supported with ESXi version. I follow this write up by [Andrew Roderos](https://andrewroderos.com/vmware-esxi-intel-nuc-12/) to create the customized ISO image needed to deploy ESXi on the NUC.  

