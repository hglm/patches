###Miscellaneous patches and files

##Linux kernel cfb optimizations

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

This patch has been included in official Raspberry Pi kernels since the
middle of 2013.

kernel-cfbfillrect-experimental.patch is an experimental patch for the
linux kernel to eliminate framebuffer memory reads during simple rectangle
fill operations at 16bpp and 32bpp. Framebuffer reads for simple fills
are totally unnecessary but can cause big a slowdown when triggered,
potentially stalling the CPU and system for some time and impacting
real-time processing. This patch will eliminate unnecessary framebuffer
reads when the pixel depth is 16bpp on 32-bit systems or either 16bpp or
32bpp on 64-bit systems. On a 32-bit system with 32bpp pixel depth,
framebuffer reads generally won't happen for regular fills so the patch
will have little effect. This version of the patch is for little-endian
systems only.

kernel-cfbfillrect-experimental-v2.patch adds support for 32-bit
big-endian systems to the previous patch. It has been reported to work
correctly on one system but otherwise lack extensive testing.

##Linux kernel ARM memory functions

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
recent ARM memset fixes (March 2013) applied. The patch set no longer
applies cleanly with up-to-date upstream kernels as of February 2015,
and would require some work to adapt.

##Improvements for the Raspberry Pi/Raspberry Pi 2 fb layer

The directory raspberrypi/fb-patchset contains a patchset for the Raspberry
Pi 1/2 Linux kernel that extends the console fb interface to use triple-
buffering, also extending the DMA-accelerated CopyArea system call. The
default text scrolling method is also changed from DMA to optimized screen
redraw for the Raspberry Pi 2. The patch set contains three patches.

The first one disables the use of DMA copy for console text scrolling on
the Raspberry Pi 2 because the new model no longer benefits, with optimized
screen redraw (as implemented using the optimized cfb functions mentioned
above) providing better performance.

The second patch extends the fb layer used by the Raspberry Pi and
Raspberry Pi 2 to provide three screen buffers instead of only one.
This facilitates smooth animation by making double or triple-buffering
possible in console applications. The DMA-accelerated copyarea is also
extended to all of the extended framebuffer.

The third patch updates the fb_copyarea function in the core fb layer to
allow the accelerated CopyArea system call access to the whole framebuffer
when triple buffering is enabled.
