# About Me
![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/8a48bbe4-f92a-4e45-999c-3aaf7f0be150)

Real Time Analytics (RTA) component in IoT Accelerator Platform
## Scope
Help customers to visualizing business data in real-time (within 60 seconds)

XDR, CDR → Kafka → Subscribed to a topic to receive XDRs → ETL flow → Elasticsearch ← Portal, REST APIs

## Technologies used
OpenShift - Two clusters; One cluster 100% RTA components while few RTA services running in other cluster

On-premises Hardware /VMWare ESXi type 1 hypervisor / RHEL / Docker and OpenShift

Elasticsearch and Kibana

Grafana & Prometheus - Monitoring 

REST APIs, Postman

# Virtualization

## What is virtualization?
Virtual representation of any hardware resources ( servers, storages, networks ) 

Abstracting physical hardware functionality into a software. 

Hypervisors (aka Virtual Machine Monitors) and virtual machines are components of virtualization. 

## Why virtualization?
Virtualization helps to use hardware resources efficiently 

Virtualization enables infrastructure-as-a-service (cloud computing services)

## (Hardware) Virtualization types
Server virtualization - refers to physical servers found in data centers, mainly x86 architectures

Storage virtualization - refers to physical storage hardware devices such as NAS, SAN found in data centers

Network virtualization - refers to computer network with elements such as switches, routers and firewalls

Multicore SoC virtualization - refers to systems-on-chip, mainly Arm and x86

## Hypervisors Classification

### Based on host:

| ![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/9795b82c-2731-4b4e-84bd-29c5bd5b312e) | ![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/6e091fcf-a5a9-4b47-9e25-24c60bc16e75) |
| --- | --- |
| Type 1 / Bare-Metal | Type 2 |
| Ex.: Xen Hypervisor, ESXi | Ex.: VirtualBox |

### Based on Virtualization type:
| --- | --- |
| Full virtualization | Paravirtualization |

# Demo - Xen Hypervisor
https://wiki.xenproject.org/wiki/Xen_Project_Software_Overview

## Step1: XenServer 8 installation on a old laptop
Doc.: https://docs.xenserver.com/en-us/xenserver/8/quick-start.html, https://wiki.xenproject.org/wiki/Xen_Project_Software_Overview, 
https://www.xenserver.com/downloads#download

![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/5c4fd7e6-0721-4f06-b3f2-b20ede07c2db)
XenServer User Interface

![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/2be1ef83-1769-4b9e-8072-df87b6dcd3ff)
DOM0 OS version of XenServer

## Step2: XenCenter 2023.3.1 installation on user laptop and add server

![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/8807a529-f4b9-442c-9bae-ff7a001da475)

## Step3: Mounting Guest OS image to XenServer

> [!NOTE]
> Option 1 - Add a shared folder with ISO files from user laptop to XenServer - Unsuccessful

![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/2d4c41a4-2bf1-43b0-8142-9ad8e8180f95)

> [!NOTE]
> Option 1 - Add a shared folder with ISO files from user laptop to XenServer - Successful

`scp` ISO image to XenServer
```shell
bdy@bdy:/mnt/c/xen_iso$ scp ubuntu-20.04.6-live-server-amd64.iso root@192.168.1.183:/media/cdrom/
.....
ubuntu-20.04.6-live-server-amd64.iso                               100% 1418MB   9.6MB/s   02:28    
```



