[![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi-pink.svg)](https://www.raspberrypi.com/)
[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)

# spark

SPARK (Small Pi Array Running Kubernetes) is a small scale edge cluster I built using [Raspberry Pi]() and the 
[clusterHAT](https://clusterhat.com/). It features a Raspberry Pi 5 as the main node, the clusterHAT over GPIO, 
4 Raspberry Pi Zero 2 W's are connected as agent nodes through the clusterHAT via the onboard USB slots. In addition to 
this it has a 500GB NVMe as a boot & control-plane storage exposed over pi's PCIe x1 FPC.

The entire cluster fits in my palm & looks something like this - ![](https://raw.githubusercontent.com/abhishekkrthakur/paris/main/images/spark.jpg)

## Vision & Inspiration
I had been wanting to learn about kubernetes for a while, but I had been putting it off; procrastinating until very recently 
when I bought a Raspberry Pi-5 from the [pimoroni website](), where I also came across the [clusterHAT](https://clusterhat.com/). 
It has been around for a while & is basically an interface for building a small scale edge cluster with Raspberry Pi's. This "sparked" my interest in learning about kubernetes and building my own 
cluster. I wanted to see what I could achieve with it, so I consolidated a bunch of my smaller projects into microservices that would use kubernetes to manage the 
build & deployment (CI/CD) process. I didn't want to get too complicated with the ClusterHAT features, so I am only using 
it as a board that manages power for the agent nodes in the edge cluster. However, the board offers a few more very useful capabilities.

## Setup
Splitting up the setup process into two steps - 
1. **Hardware**
   1. Control Node
      1. Raspberry Pi 5 8Gb RAM: This is the controller node ~ control plane.
      2. Cluster HAT v2.5: This HAT along with its script for power & ethernet management bind the pi-zero's as agents to the control node. 
      3. NVMe SSD 500GB - For boot & storage (etcd & other I/O) of the control node.
      4. 27W/5V-5A Power Supply
   2. Agent Nodes 
      1. Raspberry Pi Zero 2W: Agent Nodes. Each one of these ran a dedicated service. 
      2. 16Gb MicroSD Card: Each Pi Zero 2W has a microSD card with the OS and necessary software.
2. **Software**
   1. OS (Controller & Agents): I used Raspberry PI OS Lite (64-bit) for both. It is more optimized for RPi's compared to ubuntu.
   2. Cluster HAT Script: This script manages the GPIO pins to control power and ethernet for the Pi Zero W's.
   3. Kubernetes: I used the K3s version of kubernetes instead of K8s due to resource constraints. 


