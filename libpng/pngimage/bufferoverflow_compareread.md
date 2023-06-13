issue:https://github.com/glennrp/libpng/issues/481

# Summary

There is a  buffer overflow  in ./pngimage.c:1249 in compare_read.
Remote attackers could leverage this vulnerability to cause a denial-of-service via a crafted PNG file.

# Version
```
$ git clone https://github.com/glennrp/libpng.git
$ cd libpng && git show
commit e519af8b49f52c4ac400f50f23b48ebe36a5f4df (HEAD -> libpng16, origin/tmp/develop, origin/master, origin/libpng16, origin/HEAD)
Author: Cosmin Truta <ctruta@gmail.com>
Date:   Sun Feb 12 22:31:11 2023 +0200
...
```

# Platform
```
uname -a 
Linux user-GE40-2PC-Dragon-Eyes 5.19.0-40-generic #41~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Mar 31 16:00:14 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

# ASAN_heap-buffer-overflow 
```
==923000==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x602000002420 at pc 0x562b7a077fe5 bp 0x7ffce67c64f0 sp 0x7ffce67c64e0
READ of size 1 at 0x602000002420 thread T0
    #0 0x562b7a077fe4 in compare_read ../contrib/libtests/pngimage.c:1249
    #1 0x562b7a078ff2 in test_one_file ../contrib/libtests/pngimage.c:1493
    #2 0x562b7a079299 in do_test ../contrib/libtests/pngimage.c:1573
    #3 0x562b7a079e55 in main ../contrib/libtests/pngimage.c:1677
    #4 0x7f4c8dc29d8f in __libc_start_call_main ../sysdeps/nptl/libc_start_call_main.h:58
    #5 0x7f4c8dc29e3f in __libc_start_main_impl ../csu/libc-start.c:392
    #6 0x562b7a073994 in _start (/home/user/fuzzing/libpng/build/pngimage+0xd994)

0x602000002420 is located 0 bytes to the right of 16-byte region [0x602000002410,0x602000002420)
allocated by thread T0 here:
    #0 0x7f4c8e0b4867 in __interceptor_malloc ../../../../src/libsanitizer/asan/asan_malloc_linux.cpp:145
    #1 0x562b7a092c59 in png_malloc_base ../pngmem.c:95
    #2 0x562b7a092e3e in png_malloc ../pngmem.c:179
    #3 0x562b7a096eca in png_read_png ../pngread.c:1237
    #4 0x562b7a0758c0 in read_png ../contrib/libtests/pngimage.c:905
    #5 0x562b7a078fe1 in test_one_file ../contrib/libtests/pngimage.c:1492
    #6 0x562b7a079299 in do_test ../contrib/libtests/pngimage.c:1573
    #7 0x562b7a079e55 in main ../contrib/libtests/pngimage.c:1677
    #8 0x7f4c8dc29d8f in __libc_start_call_main ../sysdeps/nptl/libc_start_call_main.h:58

SUMMARY: AddressSanitizer: heap-buffer-overflow ../contrib/libtests/pngimage.c:1249 in compare_read
Shadow bytes around the buggy address:
  0x0c047fff8430: fa fa fd fd fa fa fd fd fa fa fd fd fa fa fd fd
  0x0c047fff8440: fa fa fd fd fa fa fd fd fa fa fd fd fa fa fd fd
  0x0c047fff8450: fa fa fd fd fa fa fd fd fa fa fd fd fa fa fd fd
  0x0c047fff8460: fa fa fd fd fa fa fd fd fa fa fd fd fa fa fd fd
  0x0c047fff8470: fa fa fd fd fa fa fd fd fa fa fd fd fa fa fd fd
=>0x0c047fff8480: fa fa 00 00[fa]fa 00 00 fa fa 00 00 fa fa 00 00
  0x0c047fff8490: fa fa 00 00 fa fa 00 00 fa fa 00 00 fa fa 00 00
  0x0c047fff84a0: fa fa 00 00 fa fa 00 00 fa fa 00 00 fa fa 00 00
  0x0c047fff84b0: fa fa 00 00 fa fa 00 00 fa fa 00 00 fa fa 00 00
  0x0c047fff84c0: fa fa 00 00 fa fa 00 00 fa fa 00 00 fa fa 00 00
  0x0c047fff84d0: fa fa 00 00 fa fa 00 00 fa fa 00 00 fa fa 00 00
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
  Shadow gap:              cc
==923000==ABORTING
```

# Steps to reproduce
```
./pngimage $poc_file
```

# POC File
[poc_png_overflow](https://github.com/gandalf4a/crash_report/blob/main/poc_png_overflow.png)

# Credit
```
Gandalf4a@QAXsrc
```
