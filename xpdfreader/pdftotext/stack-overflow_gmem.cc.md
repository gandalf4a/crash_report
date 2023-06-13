# issue
https://forum.xpdfreader.com/viewtopic.php?t=42379

# Summary
Hi there, I use my fuzzer for fuzzing the binary pdftotext, the version of XPDF is the latest (v4.04) and the operation system is Ubuntu 20.04.1 LTS and this binary crash with the following.

# Details
```
CODE: SELECT ALL

id:000150,sig:11,src:007709+005940,time:4269563917,execs:1174297775,op:splice,rep:32

AddressSanitizer:DEADLYSIGNAL
=================================================================
==1601==ERROR: AddressSanitizer: stack-overflow on address 0x7fff6d502fe8 (pc 0x0000004e8a41 bp 0x000000000006 sp 0x7fff6d502fe0 T0)
    #0 0x4e8a41 in __sanitizer::StackDepotBase<__sanitizer::StackDepotNode, 1, 20>::Put(__sanitizer::StackTrace, bool*) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a41)
    #1 0x4e8a16 in __sanitizer::StackDepotPut(__sanitizer::StackTrace) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a16)
    #2 0x44feea in __asan::Allocator::Allocate(unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType, bool) (/home/user/xpdf-4.04/xpdf/pdftotext+0x44feea)
    #3 0x44f913 in __asan::asan_malloc(unsigned long, __sanitizer::BufferedStackTrace*) (/home/user/xpdf-4.04/xpdf/pdftotext+0x44f913)
    #4 0x4c875b in malloc (/home/user/xpdf-4.04/xpdf/pdftotext+0x4c875b)
    #5 0x7f7489 in gmalloc(int) /home/user/xpdf-4.04/goo/gmem.cc:148:13
    #6 0x7f7489 in copyString(char const*) /home/user/xpdf-4.04/goo/gmem.cc:393:16
    #7 0x733e7e in Object::copy(Object*) /home/user/xpdf-4.04/xpdf/Object.cc:99:16
    #8 0x592bde in Object::arrayGet(int, Object*, int) /home/user/xpdf-4.04/xpdf/Object.h:243:19
    #9 0x592bde in Catalog::countPageTree(Object*) /home/user/xpdf-4.04/xpdf/Catalog.cc:566:12
    ...

SUMMARY: AddressSanitizer: stack-overflow (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a41) in __sanitizer::StackDepotBase<__sanitizer::StackDepotNode, 1, 20>::Put(__sanitizer::StackTrace, bool*)
==1601==ABORTING
```
# Environment
```Ubuntu 20.04.1 LTS (docker)
clang-10
xpdf-4.04
```
# POC
[poc_gmem.cc](https://github.com/gandalf4a/crash_report/blob/main/xpdfreader/pdftotext/poc_gmem.cc.zip)
# Credit
Gandalf4a (QAXsrc of China)
