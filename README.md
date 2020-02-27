# rook-arm64
Install the latest version of Rook with Ceph on ARM64

Currently you do not seem to be able to install Rook/Ceph with the CSI providers.

This is merely a set of manifests where I have disabled the CSI provider and enabled the FLEX providers to allow the latest install of Rook/Ceph.
I will look to do a helm version as well.
 
Since this is based on my playing with a Raspberry Pi4 k3s cluster I have also added an ingress for a non ssl ceph dashboard served by Traefik. 
I had trouble getting the Dashboard to work by default, probably due to my ignorance of how to correctly deal with certs in k8s/k3s.

Note these Manifest are only tested on a full 64bit Arm Ubuntu Distribution not Raspbian

https://ubuntu.com/download/server/arm

**THIS IS NOT AN INSTRUCTION HOW TO SETUP A ROOK/CEPH CLUSTER FOLLOW AT YOUR OWN RISK AND CONSULT WITH THE ROOK DOCUMENTATION OR YOU MAY LOSE DATA** 
This is merely a shortcut to getting around the issue of missing arm64 csi images and may disappear when its no longer an issue.
 
That said I have had no issues yet, you will get some errors for PG count in the dashboard so you will need bump up the number, 
again you should be using Rook's docs at this point.

I have not tuned the deployment for memory usage or features. 
The cluster will automatically use all the nodes and take over device SDA which is the top USB3 port on a Raspberry Pi4 if you have devices in both.
This is specified in lines 128-132 of cluster.yaml and you should modify this based on your own devices, again the rook docs are your thing here.  
 
The cluster created uses the USB Flash disks / usb sdd devices as bluestore block devices  

The order of application is

common.yaml

operator.yaml ( wait for the deployments/daemonsets to finish)

cluster.yaml ( wait for the deployments to finish)

storageclass.yaml

 
 This was inspired by the below which has some more instructions in setting up Rook/Ceph, the main differences are that I have provided an ingress and using block mode
 instead of dir mounts and it takes a bit more memory.  If you have little knowledge of Rook/Ceph then you maybe better staying with Dan's instructions and don't
 raise any issues with him if you try my Rook files.
 
 The exec error mentioned for Rook/Ceph is because of Rook now defaulting to CSI providers and there are issues with the availability of arm64 images
 
 https://github.com/danacr/kubernetes-the-fun-way/tree/master/01-portable-kubernetes-cluster
 
 Checkout episode 2!!
 
