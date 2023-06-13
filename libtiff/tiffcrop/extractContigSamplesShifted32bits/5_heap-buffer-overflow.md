# issue
https://gitlab.com/libtiff/libtiff/-/issues/568

https://gitlab.com/libtiff/libtiff/-/issues/569

# tiffcrop: 5 heap-buffer-overflow in extractContigSamplesShifted32bits in /tiff-4.5.0/tools/tiffcrop.c:4106,4108,4110,4112,4124
# Summary
There is a buffer overflow in extractContigSamplesShifted32bits in /tiff-4.5.0/tools/tiffcrop.c:4106,4108,4110,4112,4124. Remote attackers could leverage this vulnerability to cause a denial-of-service via a crafted tiff file.

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
# ASAN_4106_heap-buffer-overflow
```
==950398==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x6160000002e2 at pc 0x562a2c655b66 bp 0x7ffd43628040 sp 0x7ffd43628038
WRITE of size 1 at 0x6160000002e2 thread T0
    #0 0x562a2c655b65 in extractContigSamplesShifted32bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4106:24
    #1 0x562a2c61fef4 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7490:33
    #2 0x562a2c61fef4 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x562a2c61fef4 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f160c829d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7f160c829e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x562a2c54d284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x6160000002e2 is located 18 bytes to the right of 592-byte region [0x616000000080,0x6160000002d0)
allocated by thread T0 here:
    #0 0x562a2c5d00ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x562a2c61f634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x562a2c61f634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x562a2c61f634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f160c829d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4106:24 in extractContigSamplesShifted32bits
Shadow bytes around the buggy address:
  0x0c2c7fff8000: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2c7fff8010: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c2c7fff8020: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c2c7fff8030: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c2c7fff8040: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c2c7fff8050: 00 00 00 00 00 00 00 00 00 00 fa fa[fa]fa fa fa
  0x0c2c7fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2c7fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2c7fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2c7fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2c7fff80a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==950398==ABORTING
```
# ASAN_4108_heap-buffer-overflow
```
==924618==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x7f6e2bc71333 at pc 0x5640d6a96b81 bp 0x7ffe46676360 sp 0x7ffe46676358
WRITE of size 1 at 0x7f6e2bc71333 thread T0
    #0 0x5640d6a96b80 in extractContigSamplesShifted32bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4108:24
    #1 0x5640d6a60ef4 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7490:33
    #2 0x5640d6a60ef4 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x5640d6a60ef4 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f6e2c029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7f6e2c029e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x5640d698e284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x7f6e2bc71333 is located 0 bytes to the right of 252723-byte region [0x7f6e2bc33800,0x7f6e2bc71333)
allocated by thread T0 here:
    #0 0x5640d6a110ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5640d6a60634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x5640d6a60634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x5640d6a60634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f6e2c029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4108:24 in extractContigSamplesShifted32bits
Shadow bytes around the buggy address:
  0x0fee45786210: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0fee45786220: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0fee45786230: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0fee45786240: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0fee45786250: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0fee45786260: 00 00 00 00 00 00[03]fa fa fa fa fa fa fa fa fa
  0x0fee45786270: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0fee45786280: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0fee45786290: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0fee457862a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0fee457862b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==924618==ABORTING
```
# ASAN_4110_heap-buffer-overflow
```
==119742==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x6110000003bf at pc 0x55776dab3b9c bp 0x7fffdaafc040 sp 0x7fffdaafc038
WRITE of size 1 at 0x6110000003bf thread T0
    #0 0x55776dab3b9b in extractContigSamplesShifted32bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4110:24
    #1 0x55776da7def4 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7490:33
    #2 0x55776da7def4 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x55776da7def4 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fd0fe629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7fd0fe629e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x55776d9ab284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x6110000003bf is located 0 bytes to the right of 255-byte region [0x6110000002c0,0x6110000003bf)
allocated by thread T0 here:
    #0 0x55776da2e0ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x55776da7d634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x55776da7d634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x55776da7d634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fd0fe629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4110:24 in extractContigSamplesShifted32bits
Shadow bytes around the buggy address:
  0x0c227fff8020: fd fd fd fd fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8030: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c227fff8040: 00 00 00 00 00 00 00 00 00 00 00 00 00 05 fa fa
  0x0c227fff8050: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
  0x0c227fff8060: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c227fff8070: 00 00 00 00 00 00 00[07]fa fa fa fa fa fa fa fa
  0x0c227fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff80a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff80b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c227fff80c0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==119742==ABORTING
```
# ASAN_4112_heap-buffer-overflow
```
==120698==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x62700000a13f at pc 0x56471dd1fbb7 bp 0x7ffe9ac77980 sp 0x7ffe9ac77978
WRITE of size 1 at 0x62700000a13f thread T0
    #0 0x56471dd1fbb6 in extractContigSamplesShifted32bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4112:24
    #1 0x56471dce9ef4 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7490:33
    #2 0x56471dce9ef4 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x56471dce9ef4 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fcb09229d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7fcb09229e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x56471dc17284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x62700000a13f is located 0 bytes to the right of 12351-byte region [0x627000007100,0x62700000a13f)
allocated by thread T0 here:
    #0 0x56471dc9a0ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x56471dce9634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x56471dce9634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x56471dce9634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fcb09229d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4112:24 in extractContigSamplesShifted32bits
Shadow bytes around the buggy address:
  0x0c4e7fff93d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c4e7fff93e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c4e7fff93f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c4e7fff9400: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c4e7fff9410: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c4e7fff9420: 00 00 00 00 00 00 00[07]fa fa fa fa fa fa fa fa
  0x0c4e7fff9430: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c4e7fff9440: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c4e7fff9450: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c4e7fff9460: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c4e7fff9470: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==120698==ABORTING
```
# ASAN_4124_heap-buffer-overflow
```
==123142==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x604000000300 at pc 0x559884351c2c bp 0x7ffd1ed5e360 sp 0x7ffd1ed5e358
WRITE of size 1 at 0x604000000300 thread T0
    #0 0x559884351c2b in extractContigSamplesShifted32bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4124:16
    #1 0x55988431bef4 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7490:33
    #2 0x55988431bef4 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x55988431bef4 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fd955a29d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7fd955a29e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x559884249284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x604000000300 is located 0 bytes to the right of 48-byte region [0x6040000002d0,0x604000000300)
allocated by thread T0 here:
    #0 0x5598842cc0ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x55988431b634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x55988431b634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x55988431b634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fd955a29d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:4124:16 in extractContigSamplesShifted32bits
Shadow bytes around the buggy address:
  0x0c087fff8010: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
  0x0c087fff8020: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
  0x0c087fff8030: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
  0x0c087fff8040: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
  0x0c087fff8050: fa fa 00 00 00 00 07 fa fa fa 00 00 00 00 00 00
=>0x0c087fff8060:[fa]fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff80a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff80b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==123142==ABORTING
```
# Steps to reproduce
```
./tiffcrop -E right  -z 1,1,2048,2048:1,2049,2048,4097 -i -s $poc_file /tmp/foo
```
# POC File
[poc_4106]()

[poc_4108]()

[poc_4110]()

[poc_4112]()

[poc_4124]()

# Credit
Gandalf4a@QAXsrc
