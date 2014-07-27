# Xen Ceph Support

## What's working:

 Mapping of a RBD image as specified in the domU config file. Tested to be bootable etc.

## disk specification:
 (unverified, wasn't tested on my dom0)
 script=block-rbd,dev=poolname:image_name

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
     This could be done nicely with rbd map locking. The xen hotplug scripts would need "help" to correctly handle it. Right now they only know how to look at single-dom0 level and don't do it for external block scripts. (actually, seems file: type only?)


## Authors:

* Thomas Zelch
* Florian Heigl


