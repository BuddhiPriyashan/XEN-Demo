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
> Option 2 - Add a shared folder with ISO files from user laptop to XenServer - Successful


`scp` ISO image to XenServer
```shell
bdy@bdy:/mnt/c/xen_iso$ scp ubuntu-20.04.6-live-server-amd64.iso root@192.168.1.183:/media/cdrom/
.....
ubuntu-20.04.6-live-server-amd64.iso                               100% 1418MB   9.6MB/s   02:28    
```

```ssh``` into the XenServer and verify
```shell
bdy@bdy:~$ ssh root@192.168.1.183

[root@xenserver-buddhikaKandamulla ~]# ls -al /media/cdrom/
-rwxr-xr-x 1 root root 1487339520 Sep 20 17:50 ubuntu-20.04.6-live-server-amd64.iso
```
Add ```/media/cdrom``` as a ISO-type Image Storage Repository in XenServer:
```shell
[root@xenserver-buddhikaKandamulla cdrom]# xe sr-create name-label="iso" type=iso device-config:location=/media/cdrom device-config
4521de1b-eb47-c9ca-df43-81da049ee49e
```

List Storage Repositories in the XenServer:

```shell
[root@xenserver-buddhikaKandamulla cdrom]# xe sr-list
...
uuid ( RO)                : 4521de1b-eb47-c9ca-df43-81da049ee49e
          name-label ( RW): iso
    name-description ( RW):
                host ( RO): xenserver-buddhikaKandamulla
                type ( RO): iso
        content-type ( RO): iso
```
ISO Image Storage Repository XenCenter:
![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/1a253ff4-6c18-4581-9904-d627fc5af230)

## Step 4: Create a VMs
Installation Media selection step to choose ISO image during VM creation in XenCenter:
![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/d96d94a9-be5b-4ccb-88b8-2ba9650097d2)

VM successfully created:
![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/0486bf0d-1d7f-4c79-862c-eab45524a051)

## Step 5: Install Docker Daemon  https://hub.docker.com/r/misterbisson/simple-container-benchmarks/
VM with Docker installed:
![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/cde606f3-2f14-4ef7-aa5d-53e9558c1503)

## Step 6: Install Phoronix Test Suite as benchmarking tool 
https://www.phoronix-test-suite.com/?k=downloads

http://www.diva-portal.org/smash/get/diva2:1665606/FULLTEXT01.pdf

Downloading Phoronix Test Suite:
```shell
bdy@xenvmdebian3-buddhika:~$ cd benchmark_test/

# Get the installation package
bdy@xenvmdebian3-buddhika:~/benchmark_test$ wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10ite_10.8.4_all.deb

bdy@xenvmdebian3-buddhika:~/benchmark_test$ ls
phoronix-test-suite_10.8.4_all.deb
```

Installing Phoronix Test Suite:
```shell
# Install Phoronix Test Suite
$ sudo dpkg -i phoronix-test-suite_10.8.4_all.deb

# Install missing dependencies if necessary
$ sudo apt --fix-broken install
```

Selected command  line benchmark utilities to be installed in next steps:

| Hardware | Benchmark Test from ```Phoronix Test Suite``` |
| --- | --- |
| Processor | ```N-Queens``` Time to solve N-Queens problem Default problem size is 18. (18 queens on 18x18 board) |
| Memory | ```RAMspeed SMP``` Measure memory performance by Performed for COPY and ADD operations  |
| Disk | ```Dbench``` Measure disk performance by simulating file operations (create, read, write, delete) 6 and 48 parallel processes (clients) were selected |

## Step 7: Create base snapshot

Using this base snapshot two VMs (VM1 and VM2) are created In following steps.

VM1 - benchmark tests on VM

VM2 - benchmark tests on Docker container running on top of VM

![image](https://github.com/BuddhiPriyashan/XEN-Demo/assets/18088808/210afad7-a918-4d07-ac4a-bfeba52b67d0)

## Step 8: VM1 - benchmark tests on VM

### Create VM from base snapshot

### Benchmarking 

#### N-Queens

```shell
bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite install pts/n-queens

bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite run pts/n-queens-1.2.1
...
N-Queens 1.0:
    pts/n-queens-1.2.1
    Test 1 of 1
    Estimated Trial Run Count:    3
    Estimated Time To Completion: 36 Minutes [23:56 EEST]
        Started Run 1 @ 23:21:05
        Started Run 2 @ 23:26:59
        Started Run 3 @ 23:32:52

    Elapsed Time:
        349.545
        349.602
        349.483

    Average: 349.543 Seconds
    Deviation: 0.02%
    
    N-Queens 1.0
    Elapsed Time
    Seconds < Lower Is Better
    N-Queens_24Sept23 . 349.54
```
#### RAMspeed SMP

```shell
bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite install pts/ramspeed-1.4.3

bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite run pts/ramspeed-1.4.3
...
RAMspeed SMP 3.5.0:
    pts/ramspeed-1.4.3
    Memory Test Configuration
        1: Copy
        2: Scale
        3: Add
        4: Triad
        5: Average
        6: Test All Options
        ** Multiple items can be selected, delimit by a comma. **
        Type: 1,3

        1: Integer
        2: Floating Point
        3: Test All Options
        ** Multiple items can be selected, delimit by a comma. **
        Benchmark: 2
        
RAMspeed SMP 3.5.0:
    pts/ramspeed-1.4.3 [Type: Add - Benchmark: Floating Point]
    Test 1 of 2
    Estimated Trial Run Count:    3
    Estimated Test Run-Time:      6 Minutes
    Estimated Time To Completion: 12 Minutes [00:00 EEST]
        Started Run 1 @ 23:48:57
        Started Run 2 @ 23:52:43
        Started Run 3 @ 23:56:27

    Type: Add - Benchmark: Floating Point:
        8161.03
        8157.81
        8156.16

    Average: 8158.33 MB/s
    Deviation: 0.03%
    
RAMspeed SMP 3.5.0:
    pts/ramspeed-1.4.3 [Type: Copy - Benchmark: Floating Point]
    Test 2 of 2
    Estimated Trial Run Count:    3
    Estimated Time To Completion: 12 Minutes [00:11 EEST]
        Started Run 1 @ 00:00:20
        Started Run 2 @ 00:04:05
        Started Run 3 @ 00:07:50

    Type: Copy - Benchmark: Floating Point:
        7418.64
        7398.55
        7401.9

    Average: 7406.36 MB/s
    Deviation: 0.15%

    RAMspeed SMP 3.5.0
    Type: Add - Benchmark: Floating Point
    MB/s > Higher Is Better
    RAMspeed-SMP Test . 8158.33 
    
    RAMspeed SMP 3.5.0
    Type: Copy - Benchmark: Floating Point
    MB/s > Higher Is Better
    RAMspeed-SMP Test . 7406.36 
```
 
#### Dbench

```shell
bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite install pts/dbench

bdy@xenvmdebian3-buddhika:~$ phoronix-test-suite run pts/dbench-1.0.2
...
Dbench 4.0:
    pts/dbench-1.0.2
    Disk Test Configuration
        1: 1
        2: 6
        3: 12
        4: 48
        5: 128
        6: 256
        7: Test All Options
        ** Multiple items can be selected, delimit by a comma. **
        Client Count: 2,4

Dbench 4.0:
    pts/dbench-1.0.2 [Client Count: 6]
    Test 1 of 2
    Estimated Trial Run Count:    3
    Estimated Test Run-Time:      45 Minutes
    Estimated Time To Completion: 2 Hours, 15 Minutes [02:43 EEST] 
        Started Run 1 @ 00:28:32
        Started Run 2 @ 00:40:36
        Started Run 3 @ 00:52:41

    Client Count: 6:
        255.977
        257.834
        267.091

    Average: 260.301 MB/s
    Deviation: 2.29%

Dbench 4.0:
    pts/dbench-1.0.2 [Client Count: 48]
    Test 2 of 2
    Estimated Trial Run Count:    3
    Estimated Test Run-Time:      37 Minutes
    Estimated Time To Completion: 1 Hour, 13 Minutes [02:16 EEST]
        Started Run 1 @ 01:04:52
        Started Run 2 @ 01:16:58
        Started Run 3 @ 01:29:04

    Client Count: 48:
        52.143
        52.385
        53.8318

    Average: 52.7866 MB/s
    Deviation: 1.73%
    
    Dbench 4.0
    Client Count: 6
    MB/s > Higher Is Better
    dbench disk benchmark test on XenVM . 260.30 
    
    Dbench 4.0
    Client Count: 48
    MB/s > Higher Is Better
    dbench disk benchmark test on XenVM . 52.79
```



