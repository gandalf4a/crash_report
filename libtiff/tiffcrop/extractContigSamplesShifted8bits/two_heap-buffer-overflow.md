# issue
https://gitlab.com/libtiff/libtiff/-/issues/563

https://gitlab.com/libtiff/libtiff/-/issues/562

# tiffcrop: heap-buffer-overflow in /tiff-4.5.0/tools/tiffcrop.c:3760,3773 in extractContigSamplesShifted8bits
# Summary
There is a buffer overflow in /tiff-4.5.0/tools/tiffcrop.c:3760:24 in extractContigSamplesShifted8bits. Remote attackers could leverage this vulnerability to cause a denial-of-service via a crafted tiff file.

# Version
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
# ASAN_heap-buffer-overflow_3760
```
==1115154==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61d000001311 at pc 0x55ca43a22107 bp 0x7ffee1c7fe40 sp 0x7ffee1c7fe38
READ of size 1 at 0x61d000001311 thread T0
    #0 0x55ca43a22106 in extractContigSamplesShifted8bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3760:24
    #1 0x55ca439ede40 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7452:37
    #2 0x55ca439ede40 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x55ca439ede40 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fa174629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7fa174629e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x55ca4391b284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x61d000001311 is located 45 bytes to the right of 2148-byte region [0x61d000000a80,0x61d0000012e4)
allocated by thread T0 here:
    #0 0x55ca4399e0ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x55ca439ed634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x55ca439ed634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x55ca439ed634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fa174629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3760:24 in extractContigSamplesShifted8bits
Shadow bytes around the buggy address:
  0x0c3a7fff8210: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff8220: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff8230: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff8240: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff8250: 00 00 00 00 00 00 00 00 00 00 00 00 04 fa fa fa
=>0x0c3a7fff8260: fa fa[fa]fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8270: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8280: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8290: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff82a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff82b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==1115154==ABORTING
```
# ASAN_heap-buffer-overflow_3773
```
==886338==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61100000010f at pc 0x556b9b484125 bp 0x7ffeed816be0 sp 0x7ffeed816bd8
WRITE of size 1 at 0x61100000010f thread T0
    #0 0x556b9b484124 in extractContigSamplesShifted8bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3773:16
    #1 0x556b9b44fe40 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7452:37
    #2 0x556b9b44fe40 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x556b9b44fe40 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f17a7e29d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7f17a7e29e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x556b9b37d284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x61100000010f is located 0 bytes to the right of 207-byte region [0x611000000040,0x61100000010f)
allocated by thread T0 here:
    #0 0x556b9b4000ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x556b9b44f634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x556b9b44f634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x556b9b44f634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f17a7e29d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3773:16 in extractContigSamplesShifted8bits
Shadow bytes around the buggy address:
  0x0c227fff7fd0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c227fff7fe0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c227fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c227fff8000: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
  0x0c227fff8010: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c227fff8020: 00[07]fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8030: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8040: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==886338==ABORTING
```

# Steps to reproduce
```
./tiffcrop -E right  -z 1,1,2048,2048:1,2049,2048,4097 -i -s $poc_file /tmp/foo
```
# POC File
[poc_of_3760](https://github.com/gandalf4a/crash_report/blob/main/libtiff/tiffcrop/extractContigSamplesShifted8bits/poc_of_3760)

[poc_of_3773](https://github.com/gandalf4a/crash_report/blob/main/libtiff/tiffcrop/extractContigSamplesShifted8bits/poc_of_3773)

# Credit
Gandalf4a@QAXsrc
