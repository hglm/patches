Miscellaneous patches.

hglm-sunxifb-patch-v0.6.patch is a patch for the sunxifb driver that implements a
number of possible performance improvements. Version 0.6 fixes the PutImage
optimization that was accidently disabled.

benchimagemark.tar.gz is a slightly modified version of a program I found
on the web to measure off-screen to screen copy throughput using various methods
(xPutImage, xShmPutImage, shared memory pixmaps).
