# Xen Ceph Support

The goal of this project is to work towards a widely available, simple and fast integration of 
Ceph in Xen.
The most direct way towards this is via a custom block script in /etc/xen/scripts.
The best-performing way would be a LIBRADOS based blockback driver, which is more complex to tackle.
Starting there would result in yet another powerful but non-upstreamed dead Xen addon. 
Instead we go for the simplest option first and will use the time gained to press for upstreaming.


Instead we went for what __will immediately work for everyone__:


1. RDB Image 
2. block-rbd script 
3. disk specification 

=> __domU running natively on Ceph__



## What's working:

 Mapping of a RBD image as specified in the domU config file. Tested to be bootable etc.

## Config:

Syntax example:

`disk = [ 'script=block-rbd,vdev=xvdN,target=poolname:image_name' ]`

To define __xvda__ as im __ceph_test__ in pool __rbd__ :

`disk = [ 'script=block-rbd,vdev=xvda,target=rbd:ceph-test' ]`


## PV-Grub config:

### Syntax example for grub0.97:

    kernel = "/usr/lib/xen/boot/pv-grub-x86_64.gz"
    extra  = "(hd0,0)/boot/grub/menu.lst"

### Warning: 

PV-Grub can only boot "legacy Grub". On distros using Grub2 (Wheezy, RHEL7) you need to create a valid menu.lst

  
## What's not working:

* PyGrub


## What's planned:

* PyGrub support
* PV-Grub support


## Optional:

*  Mild auto-search of images - if name matches the domU name, it might be fair game. Probably no good since most domU have multiple block devices.

*  Being able to auto-clone an image off a base rbd image
     This could be used to implement VDI autocloning using Xen. It would be implemented as an optional feature triggered via extra flags.

*  Implement proper support for __w,r,r!,w!__ by asking Ceph about single-/multiattachment (so, cluster-wide locking)
     This could be done nicely with __rbd map__, which has locking support. The xen hotplug scripts would need "help" to correctly handle it. Right now they only know how to look at single-dom0 level and don't do it for external block scripts. (actually, seems file: type only?)


## Wishlist, but would need contributors

* Librados blockback (would work best, maybe fastest, storage domU support)
* qdisk Librados (has better SCSI emulation)


## Other solutions

### blktap / RBD (not very active):
 info: http://thr3ads.net/xen-devel/2013/04/2289675-Xen-blktap-driver-for-Ceph-RBD-Anybody-wants-to-test-p
 source: https://github.com/smunaut/blktap/tree/rbd


## Authors:

* Thomas Zelch
* Florian Heigl



