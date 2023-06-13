# issue
https://gitlab.com/libtiff/libtiff/-/issues/565

https://gitlab.com/libtiff/libtiff/-/issues/566

https://gitlab.com/libtiff/libtiff/-/issues/567

# tiffcrop: heap-buffer-overflow in /tiff-4.5.0/tools/tiffcrop.c:3998,3984,3982 in extractContigSamplesShifted24bits
# Summary
There is a buffer overflow in /tiff-4.5.0/tools/tiffcrop.c:3998:16 in extractContigSamplesShifted24bits. Remote attackers could leverage this vulnerability to cause a denial-of-service via a crafted tiff file.

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
# ASAN_heap-buffer-overflow_3998
```
==944090==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x6130000001c0 at pc 0x5593e03fe175 bp 0x7ffe4274cf30 sp 0x7ffe4274cf28
WRITE of size 1 at 0x6130000001c0 thread T0
    #0 0x5593e03fe174 in extractContigSamplesShifted24bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3998:16
    #1 0x5593e03c8e98 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7476:33
    #2 0x5593e03c8e98 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x5593e03c8e98 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f7033029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7f7033029e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x5593e02f6284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x6130000001c0 is located 1 bytes to the right of 383-byte region [0x613000000040,0x6130000001bf)
allocated by thread T0 here:
    #0 0x5593e03790ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5593e03c8634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x5593e03c8634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x5593e03c8634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f7033029d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3998:16 in extractContigSamplesShifted24bits
Shadow bytes around the buggy address:
  0x0c267fff7fe0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c267fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c267fff8000: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
  0x0c267fff8010: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c267fff8020: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c267fff8030: 00 00 00 00 00 00 00 07[fa]fa fa fa fa fa fa fa
  0x0c267fff8040: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c267fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c267fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c267fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c267fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==944090==ABORTING
```
# ASAN_heap-buffer-overflow_3984
```
==943382==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61b000001ba7 at pc 0x564675acd104 bp 0x7ffcf480dd10 sp 0x7ffcf480dd08
WRITE of size 1 at 0x61b000001ba7 thread T0
    #0 0x564675acd103 in extractContigSamplesShifted24bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3984:24
    #1 0x564675a97e98 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7476:33
    #2 0x564675a97e98 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x564675a97e98 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fce1c629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7fce1c629e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x5646759c5284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x61b000001ba7 is located 0 bytes to the right of 1575-byte region [0x61b000001580,0x61b000001ba7)
allocated by thread T0 here:
    #0 0x564675a480ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x564675a97634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x564675a97634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x564675a97634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7fce1c629d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3984:24 in extractContigSamplesShifted24bits
Shadow bytes around the buggy address:
  0x0c367fff8320: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c367fff8330: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c367fff8340: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c367fff8350: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c367fff8360: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c367fff8370: 00 00 00 00[07]fa fa fa fa fa fa fa fa fa fa fa
  0x0c367fff8380: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c367fff8390: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c367fff83a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c367fff83b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c367fff83c0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==943382==ABORTING

```
# ASAN_heap-buffer-overflow_3982
```
==933263==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x6180000007b0 at pc 0x5640a31420ea bp 0x7ffe60fe0870 sp 0x7ffe60fe0868
WRITE of size 1 at 0x6180000007b0 thread T0
    #0 0x5640a31420e9 in extractContigSamplesShifted24bits /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3982:24
    #1 0x5640a310ce98 in extractCompositeRegions /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:7476:33
    #2 0x5640a310ce98 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:8504:13
    #3 0x5640a310ce98 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f4eb2229d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    #5 0x7f4eb2229e3f in __libc_start_main csu/../csu/libc-start.c:392:3
    #6 0x5640a303a284 in _start (/home/user/fuzzing_tiff/install/bin/tiffcrop+0xd7284) (BuildId: b74ccf7dfc4e9a79)

0x6180000007b0 is located 9 bytes to the right of 807-byte region [0x618000000480,0x6180000007a7)
allocated by thread T0 here:
    #0 0x5640a30bd0ce in malloc (/home/user/fuzzing_tiff/install/bin/tiffcrop+0x15a0ce) (BuildId: b74ccf7dfc4e9a79)
    #1 0x5640a310c634 in _TIFFmalloc /home/user/fuzzing_tiff/tiff-4.5.0/libtiff/tif_unix.c:326:13
    #2 0x5640a310c634 in processCropSelections /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c
    #3 0x5640a310c634 in main /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:2807:21
    #4 0x7f4eb2229d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/user/fuzzing_tiff/tiff-4.5.0/tools/tiffcrop.c:3982:24 in extractContigSamplesShifted24bits
Shadow bytes around the buggy address:
  0x0c307fff80a0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c307fff80b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c307fff80c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c307fff80d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c307fff80e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c307fff80f0: 00 00 00 00 07 fa[fa]fa fa fa fa fa fa fa fa fa
  0x0c307fff8100: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8110: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8120: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8130: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8140: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==933263==ABORTING
```
# Steps to reproduce
```
./tiffcrop -E right  -z 1,1,2048,2048:1,2049,2048,4097 -i -s $poc_file /tmp/foo
```
# POC File
[poc_of_3982](https://github.com/gandalf4a/crash_report/blob/main/libtiff/tiffcrop/extractContigSamplesShifted24bits/poc_of_3982)

[poc_of_3984](https://github.com/gandalf4a/crash_report/blob/main/libtiff/tiffcrop/extractContigSamplesShifted24bits/poc_of_3984)

[poc_of_3998](https://github.com/gandalf4a/crash_report/blob/main/libtiff/tiffcrop/extractContigSamplesShifted24bits/poc_of_3998)

# Credit
Gandalf4a@QAXsrc
