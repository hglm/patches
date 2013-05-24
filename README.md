Miscellaneous patches.

hglm-sunxifb-patch-v0.7.patch is a patch for the sunxifb driver that implements a
number of possible performance improvements.

Changes in v0.8:

- Fix constraint for 16bpp blit in 32-bit mode which could potentially cause
  stability problems.

Changes in v0.7:

- Get rid of DRAWABLE_WINDOW checks; also handle off-screen cases for the drawing
  primitives that are accelerated with G2D, using pixman or G2D if the destination
  is inside the framebuffer. This should improve performance especially for
  PutImage into off-screen windows or pixmaps.

- Fix vertical line corruption seen at 16bpp by adding more stringent constraints
  for source and destination coordinates for 16bpp blit in 32-bit mode.

At the moment the patched sunxifb driver passes without failures the X Test Suite
when running the tests XCopyArea, XFillRect, XFillRectangles and XPutImage, both
at 32bpp and 16bpp.

sunxifb-performance-tests gives some detail on performance improvements implemented
by the patch using benchmarks such as x11perf.

benchimagemark.tar.gz is a slightly modified version of a program I found
on the web to measure off-screen to screen copy throughput using various methods
(xPutImage, xShmPutImage, shared memory pixmaps).
