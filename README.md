Miscellaneous patches and files

kernel-cfbimageblt.patch is a patch for the Linux kernel to significantly
speed up framebuffer console text drawing. It has been tested on a 3.4.x
kernel used on the ARM-based Allwinner platform and on a 3.6.x kernel on
the ARM-based Raspberry Pi platform. It should be platform independent
although it has only been tested on ARM platforms. The biggest benefit of
using this patch will be gained on systems with a relatively slower CPU but
with a decent amount of framebuffer bandwidth, such as ARM-based devices.

Tested platforms:

- Raspberry Pi with 3.6.y kernel.
- Allwinner A10 linux-sunxi platform with 3.4.x kernel.

kernel-armv6v7-mem-funcs.patch is a work-in-progress experimental patch
for the Linux kernel to bring up to date the ARM-optimized memory copy
and fill functions, which are for a large part still optimized for older
ARM platforms and do not perform optimally on more recent platforms.
The patch supports both armv6 (line size 32 bytes) and armv7 (line size
64 bytes) platforms. The patch implements changes to the prefetch
strategy and other optimizations. On certain platforms, such as the
 armv6-based Raspberry Pi, a significant speed improvement (on the order
of 70%) is seen for important functions like copy_page and larger size
memcpy. On armv7-based platforms a smaller, but nonetheless not
insignificant benefit is seen. The changes keep using the regular
register file, without using vfp or NEON, minimizing overhead. In
its current form the patch may break older ARM architectures or (more
likely) cause a performance regression for them. Note that the specific
optimizations that are optimal for a platform may depend highly on the
details of the particular SOC implementation and may not be the same for
platforms sharing the same ARM architecture.

As a side-effect, the ARM memset function now properly returns the
original destination in r0, which may help fix issues relating to
compiling ARM kernels with gcc versions greater than 4.7. However,
newer kernels (such as stable 3.9.x and kernels backported wit the
fix) already contain a fix for this; the patch will not apply cleanly
in this case. A version that can be applied cleanly against these
kernels is available as kernel-recent-armv6v7-mem-funcs.patch.

Tested platforms:

- Raspberry Pi (armv6) with 3.6.y kernel
- Raspberry Pi with 3.9.y kernel, gcc 4.8
- Allwinner A10 linux-sunxi platform with 3.4.43 kernel, gcc 4.7 and 4.8

sunxifb/ holds mostly obsolete patches for the xf86-video-sunxifb driver
corresponding to the various-optimizations branch.

sunxifb/sunxifb-performance-tests gives some detail on performance
improvements implemented by the various-optimizations branch of the forked
xf86-video-sunxi repository using benchmarks such as x11perf.

benchimagemark.tar.gz is a slightly modified version of a program I found
on the web to measure off-screen to screen copy throughput using various
methods (xPutImage, xShmPutImage, shared memory pixmaps).

