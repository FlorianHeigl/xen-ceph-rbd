


Xen Ceph Support


What's working:

Mapping of a RBD image as specified in the domU config file.
Tested to be bootable etc.

Specification:


What's not working:
-------------------

PyGrub


What's untested:
----------------

PV-Grub   (should just work)



What's planned:
---------------

PyGrub support
PV-Grub support 
Mild auto-search of images like
	for image in images:
 		if image == vm_hostname

Being able to auto-clone an image off a base rbd image
Implement proper support for w,r,r!,w! by asking Ceph about single-/multiattachment
(so, cluster-wide locking)
