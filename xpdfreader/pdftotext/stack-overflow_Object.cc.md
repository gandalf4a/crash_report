# issue
https://forum.xpdfreader.com/viewtopic.php?t=42376

# Summary
Hi there, I use my fuzzer for fuzzing the binary pdftotext, the version of XPDF is the latest (v4.04) and the operation system is Ubuntu 20.04.1 LTS and this binary crash with the following.
# Details
```
CODE: SELECT ALL

id:000160,sig:11,src:004615+000692,time:5049175234,execs:1403039222,op:splice,rep:16

AddressSanitizer:DEADLYSIGNAL
=================================================================
==1648==ERROR: AddressSanitizer: stack-overflow on address 0x7fff418e8f18 (pc 0x0000004c7b36 bp 0x7fff418e9760 sp 0x7fff418e8f20 T0)
    #0 0x4c7b36 in __asan_memcpy (/home/user/xpdf-4.04/xpdf/pdftotext+0x4c7b36)
    #1 0x733d5b in Object::copy(Object*) /home/user/xpdf-4.04/xpdf/Object.cc:81:8
    #2 0x7cc054 in XRef::fetch(int, int, Object*, int) /home/user/xpdf-4.04/xpdf/XRef.cc
    #3 0x592bde in Object::arrayGet(int, Object*, int) /home/user/xpdf-4.04/xpdf/Object.h:243:19
    #4 0x592bde in Catalog::countPageTree(Object*) /home/user/xpdf-4.04/xpdf/Catalog.cc:566:12
    ...

SUMMARY: AddressSanitizer: stack-overflow (/home/user/xpdf-4.04/xpdf/pdftotext+0x4c7b36) in __asan_memcpy
==1648==ABORTING
```

# Environment
```
Ubuntu 20.04.1 LTS (docker)
clang-10
xpdf-4.04
```

# POC
[poc_Object.cc](https://github.com/gandalf4a/crash_report/blob/main/xpdfreader/pdftotext/poc_Object.cc.zip)

# Credit
Gandalf4a (QAXsrc of China)
