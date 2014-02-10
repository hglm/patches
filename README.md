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

kernel-cfbfillrect-experimental.patch is an experimental patch for the
linux kernel to eliminate framebuffer memory reads during simple rectangle
fill operations at 16bpp and 32bpp. Framebuffer reads for simple fills
are totally unnecessary but can cause big a slowdown when triggered. When
cfbfillrect is called repeatedly for small areas, and the framebuffer
depth is 16bpp on 32-bit systems or any depth on 64-bit systems, this
patch should provide improved performance.

kernel-cfbfillrect-experimental-v2.patch add untested support for 32-bit
big-endian systems to the previous patch

The arm-mem-funcs directory contains a work-in-progress experimental
patch set for the Linux kernel to bring up to date the ARM-optimized
memory copy and fill functions, which are for a large part still
optimized for older ARM platforms and do not perform optimally on more
recent platforms. The changes supports both armv6 (line size 32 bytes)
and armv7 (line size 64 bytes) platforms. The patches implement changes
to the prefetch strategy and other optimizations. On certain platforms,
such as the  armv6-based Raspberry Pi, a significant speed improvement
(on the order of 70%) is seen for important functions like copy_page and
larger size memcpy. On armv7-based platforms a smaller, but nonetheless
not insignificant benefit is seen. The changes keep using the regular
register file, without using vfp or NEON, minimizing overhead.

Note that the specific optimizations that are optimal for a platform
may depend highly on the details of the particular SOC implementation
and may not be the same for platforms sharing the same ARM architecture.

Current status: Optimized copy_page, memset and memzero are provided.
Optimized memcpy, copy_from_user and copy_to_user are still work in
progress. However, the test-arm-kernel-memcpy repository contains
userspace versions for benchmarking and validation of the optimized
functions are does include preliminary implementations of memcpy.

To apply cleanly the patch set requires an up-to-date kernel with the
recent ARM memset fixes (March 2013) applied.

Tested platforms:

- Allwinner A10 linux-sunxi platform with 3.4.43 kernel, gcc 4.8

sunxifb/ holds mostly obsolete patches for the xf86-video-sunxifb driver
corresponding to the various-optimizations branch.

sunxifb/sunxifb-performance-tests gives some detail on performance
improvements implemented by the various-optimizations branch of the forked
xf86-video-sunxi repository using benchmarks such as x11perf.

benchimagemark.tar.gz is a slightly modified version of a program I found
on the web to measure off-screen to screen copy throughput using various
methods (xPutImage, xShmPutImage, shared memory pixmaps).

