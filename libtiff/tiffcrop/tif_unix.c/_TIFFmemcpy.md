issue:https://gitlab.com/libtiff/libtiff/-/issues/573
# tiffcrop: heap-buffer-overflow and heap-use-after-free and SIGSEGV in /tiff-4.5.0/libtiff/tif_unix.c:345:5 in _TIFFmemcpy

# Summary
There is a buffer overflow and heap UAF and SIGSEGV in _TIFFmemcpy in libtiff/tif_unix.c:345:5. Remote attackers could leverage this vulnerability to cause a denial-of-service via a crafted tiff file.

# Version
from：https://download.osgeo.org/libtiff/tiff-4.5.0.tar.gz
```
./tiffcrop -v
Library Release: LIBTIFF, Version 4.5.0
Copyright (c) 1988-1996 Sam Leffler
Copyright (c) 1991-1996 Silicon Graphics, Inc.
Tiffcp code: Copyright (c) 1988-1997 Sam Leffler
           : Copyright (c) 1991-1997 Silicon Graphics, Inc
Tiffcrop additions: Copyright (c) 2007-2010 Richard Nolde
```

# Platform
```
uname -a 
Linux user-GE40-2PC-Dragon-Eyes 5.19.0-40-generic #41~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Mar 31 16:00:14 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

# ASAN_heap-buffer-overflow
```
==1042821==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x603000000265 at pc 0x555b676204aa bp 0x7ffce006c330 sp 0x7ffce006bb00
WRITE of size 2 at 0x603000000265 thread T0
    #0 0x555b676204a9 in __asan_memcpy (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x1594a9) (BuildId: b74ccf7dfc4e9a79)
    #1 0x555b676a1e86 in _TIFFmemcpy /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:345:5
    #2 0x555b676a1e86 in extractContigSamplesBytes /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3215:9
    #3 0x555b67670ddc in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7439:33
    #4 0x555b67670ddc in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #5 0x555b67670ddc in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #6 0x7fbcdc029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #7 0x7fbcdc029e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #8 0x555b6759e284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x603000000265 is located 0 bytes to the right of 21-byte region [0x603000000250,0x603000000265)
allocated by thread T0 here:
    #0 0x555b676210ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x555b67670634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x555b67670634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x555b67670634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fbcdc029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x1594a9) (BuildId: b74ccf7dfc4e9a79) in __asan_memcpy
Shadow bytes around the buggy address:
  0x0c067fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff8000: fa fa 00 00 00 fa fa fa 00 00 00 00 fa fa 00 00
  0x0c067fff8010: 00 00 fa fa 00 00 00 00 fa fa 00 00 00 00 fa fa
  0x0c067fff8020: 00 00 00 00 fa fa 00 00 00 00 fa fa 00 00 00 00
  0x0c067fff8030: fa fa 00 00 00 00 fa fa 00 00 00 fa fa fa fd fd
=>0x0c067fff8040: fd fa fa fa 00 00 05 fa fa fa 00 00[05]fa fa fa
  0x0c067fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
```

# ASAN_heap-use-after-free
```
==1090608==ERROR: AddressSanitizer: heap-use-after-free on address 0x7f6c895044c8 at pc 0x5639f96734aa bp 0x7ffdf8314ad0 sp 0x7ffdf83142a0
WRITE of size 31780 at 0x7f6c895044c8 thread T0
    #0 0x5639f96734a9 in __asan_memcpy (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x1594a9) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5639f96f4e86 in _TIFFmemcpy /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:345:5
    #2 0x5639f96f4e86 in extractContigSamplesBytes /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3215:9
    #3 0x5639f96c3ddc in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7439:33
    #4 0x5639f96c3ddc in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #5 0x5639f96c3ddc in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #6 0x7f6c8b829d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #7 0x7f6c8b829e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #8 0x5639f95f1284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x7f6c895044c8 is located 19656 bytes inside of 1048576-byte region [0x7f6c894ff800,0x7f6c895ff800)
freed by thread T0 here:
    #0 0x5639f9673e22 in free (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x159e22) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5639f9773cdd in _TIFFfree /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:337:27
    #2 0x5639f9773cdd in _TIFFfreeExt /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_open.c:165:5
    #3 0x5639f9773cdd in TIFFReadDirEntryArrayWithLimit /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:1341:17
    #4 0x5639f976238d in TIFFReadDirEntryArray /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:1378:12
    #5 0x5639f976238d in TIFFReadDirEntryShortArray /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:1765:11
    #6 0x5639f9755a32 in TIFFFetchNormalTag /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:6796:19
    #7 0x5639f9744fe4 in TIFFReadDirectory /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:4528:27
    #8 0x5639f987f50a in TIFFClientOpenExt /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_open.c:625:17
    #9 0x5639f98d6fc5 in TIFFFdOpenExt /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:209:11
    #10 0x5639f98d6fc5 in TIFFOpenExt /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:255:11

previously allocated by thread T0 here:
    #0 0x5639f96744f6 in realloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a4f6) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5639f977491e in _TIFFrealloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:339:51
    #2 0x5639f977491e in _TIFFreallocExt /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_open.c:158:12
    #3 0x5639f977491e in TIFFReadDirEntryDataAndRealloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_dirread.c:1237:24

SUMMARY: AddressSanitizer: heap-use-after-free (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x1594a9) (BuildId: b74ccf7dfc4e9a79) in __asan_memcpy
Shadow bytes around the buggy address:
  0x0fee11298840: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee11298850: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee11298860: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee11298870: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee11298880: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0fee11298890: fd fd fd fd fd fd fd fd fd[fd]fd fd fd fd fd fd
  0x0fee112988a0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee112988b0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee112988c0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee112988d0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0fee112988e0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
```
# ASAN_SIGSEGV
```
==1096528==ERROR: AddressSanitizer: SEGV on unknown address 0x633000040000 (pc 0x7f3095ac4b63 bp 0x7ffc9ca4b870 sp 0x7ffc9ca4b038 T0)
==1096528==The signal is caused by a WRITE memory access.
    #0 0x7f3095ac4b63  string/../sysdeps/x86_64/multiarch/memmove-vec-unaligned-erms.S:540
    #1 0x55603f7e44e1 in __asan_memcpy (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x1594e1) (BuildId: b74ccf7dfc4e9a79)
    #2 0x55603f865e86 in _TIFFmemcpy /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:345:5
    #3 0x55603f865e86 in extractContigSamplesBytes /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3215:9
    #4 0x55603f834ddc in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7439:33
    #5 0x55603f834ddc in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #6 0x55603f834ddc in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #7 0x7f3095a29d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #8 0x7f3095a29e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #9 0x55603f762284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV string/../sysdeps/x86_64/multiarch/memmove-vec-unaligned-erms.S:540 
==1096528==ABORTING
```
# Steps to reproduce
```
./tiffcrop -E right  -z 1,1,2048,2048:1,2049,2048,4097 -i -s $poc_file /tmp/foo
```

# POC File
poc_offerflow

poc_SEGV 

poc_UAF

# Credit
Gandalf4a@QAXsrc
