[![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi-pink.svg)](https://www.raspberrypi.com/)
[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)

# spark
SPARK (Small Pi Array Running Kubernetes) is a small scale edge cluster I built with [Raspberry Pi](http://raspberrypi.com) 
and [clusterHAT](https://clusterhat.com/). The entire cluster fits in my palm & looks something like this - 
![](https://raw.githubusercontent.com/abhishekkrthakur/paris/main/images/spark.jpg).

## Setup
Splitting up the setup process into two steps - 
1. **Hardware**
   1. Controller Node 
      - Raspberry Pi 5 8Gb RAM: This is the controller node ~ control plane. 
      - Cluster HAT v2.5: This HAT along with its script for power & ethernet management bind the pi-zero's as agents to the control node. 
      - NVMe SSD 500GB - For boot & storage (etcd & other I/O) of the control node. 
      - NVMe SSD 1TB - To serve as a local data & file storage for the system. 
      - 27W/5V-5A Power Supply.
   2. Agent Nodes 
      - Raspberry Pi Zero 2W: Agent Nodes. Each one of these ran a dedicated service. 
      - 16Gb MicroSD Card: Each Pi Zero 2W has a microSD card with the OS and necessary software.
2. **Software**
   1. Controller Node:
      - Operating System: 64-bit Raspberry Pi OS Lite. This allows more room for K3s & etcd to run.
      - Cluster HAT Script: This script manages the GPIO pins to control power and ethernet for the Pi Zero W's.
      - Kubernetes: Rancher K3s kubernetes (controller). It's more optimized for resource constrained edge devices.
      - etcd: I used the embedded etcd packaged with K3s.
      - Telemetry & Monitoring: I set up Prometheus & Grafana to monitor the cluster's performance and resource usage.
   2. Agent Nodes:
      - Operating System: 64-bit Raspberry Pi OS Lite. Minimal for headless pi's.
      - Cluster HAT Script: This script manages the GPIO pins to control power and ethernet on Pi Zero W's. 
      - Kubernetes: Rancher K3s (worker nodes). It works well for resource constrained Raspberry Pi Zero 2W.
      - Python Microservices: I deployed the 4 data-processing modules of my [news-bulletin](https://github.com/gagansopori/news-bulletin) project on the agent nodes. 

## vision & inspiration
I had been wanting to learn about kubernetes for a while, but I had been putting it off; procrastinating until very recently 
when I bought a Raspberry Pi-5 from the [pimoroni website](), where I also came across the [clusterHAT](https://clusterhat.com/). 
It has been around for a while & is basically an interface for building a small scale edge cluster with Raspberry Pi's. This "sparked" my interest in learning about kubernetes and building my own 
cluster. I wanted to see what I could achieve with it, so I consolidated a bunch of my smaller projects into microservices that would use kubernetes to manage the 
build & deployment (CI/CD) process. I didn't want to get too complicated with the ClusterHAT features, so I am only using 
it as a board that manages power for the agent nodes in the edge cluster. However, the board offers a few more very useful capabilities.

It features a Raspberry Pi 5 as the main node, the clusterHAT over GPIO, 4 Raspberry 
Pi Zero 2 W's are connected as agent nodes through the clusterHAT via the onboard USB slots. In addition to 
this it has a 500GB NVMe as a boot & control-plane storage exposed over pi's PCIe x1 FPC.