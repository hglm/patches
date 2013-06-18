Miscellaneous patches and files

sunxifb/ holds patches for the xf86-video-sunxifb driver corresponding to the
various-optimizations branch.

sunxifb/sunxifb-performance-tests gives some detail on performance improvements implemented
by the various-optimizations branch of the forked xf86-video-sunxi repository using
benchmarks such as x11perf.

benchimagemark.tar.gz is a slightly modified version of a program I found
on the web to measure off-screen to screen copy throughput using various methods
(xPutImage, xShmPutImage, shared memory pixmaps).

kernel-cfbimageblt.patch is a patch for the 3.4.x kernel used on the Allwinner platform
to speed up framebuffer console text drawing.

