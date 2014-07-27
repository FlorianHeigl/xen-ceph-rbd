# Xen Ceph Support

The goal of this project is to work towards a widely available, simple and fast integration of 
Ceph in Xen.
The most direct way towards this is via a custom block script in /etc/xen/scripts.
The best-performing way would be a LIBRADOS based blockback driver, which is more complex to tackle.
Starting there would result in yet another powerful but non-upstreamed dead Xen addon. 


Instead we went for what __will immediately work for everyone__:


1. RDB Image 
2. block-rbd script 
3. disk specification 

=> __domU running natively on Ceph__



## What's working:

 Mapping of a RBD image as specified in the domU config file. Tested to be bootable etc.

## Config:
 (unverified, wasn't tested on my dom0)

`disk = [ 'script=block-rbd,dev=poolname:image_name,target=xvda' ]`

## What's not working:

* PyGrub


## What's untested:

* PV-Grub   (should just work)


## What's planned:

* PyGrub support
* PV-Grub support


## Optional:

*  Mild auto-search of images - if name matches the domU name, it might be fair game. Probably no good since most domU have multiple block devices.

*  Being able to auto-clone an image off a base rbd image
     This could be used to implement VDI autocloning using Xen. It would be implemented as an optional feature triggered via extra flags.

*  Implement proper support for __w,r,r!,w!__ by asking Ceph about single-/multiattachment (so, cluster-wide locking)
     This could be done nicely with __rbd map__, which has locking support. The xen hotplug scripts would need "help" to correctly handle it. Right now they only know how to look at single-dom0 level and don't do it for external block scripts. (actually, seems file: type only?)


## Authors:

* Thomas Zelch
* Florian Heigl


