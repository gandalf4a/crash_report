# issue
https://forum.xpdfreader.com/viewtopic.php?t=42377

# Summary
Hi there, I use my fuzzer for fuzzing the binary pdftotext, the version of XPDF is the latest (v4.04) and the operation system is Ubuntu 20.04.1 LTS and this binary crash with the following.

# Details
```
CODE: SELECT ALL

id:000019,sig:11,src:003974,time:15599291,execs:3612044,op:havoc,rep:8

AddressSanitizer:DEADLYSIGNAL
=================================================================
==2685==ERROR: AddressSanitizer: stack-overflow on address 0x7fff7e8f0ff8 (pc 0x0000004e8a39 bp 0x000000000150 sp 0x7fff7e8f1000 T0)

    #0 0x4e8a39 in __sanitizer::StackDepotBase<__sanitizer::StackDepotNode, 1, 20>::Put(__sanitizer::StackTrace, bool*) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a39)
    #1 0x4e8a16 in __sanitizer::StackDepotPut(__sanitizer::StackTrace) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a16)
    #2 0x44feea in __asan::Allocator::Allocate(unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType, bool) (/home/user/xpdf-4.04/xpdf/pdftotext+0x44feea)
    #3 0x450749 in __asan::asan_memalign(unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType) (/home/user/xpdf-4.04/xpdf/pdftotext+0x450749)
    #4 0x4f8402 in operator new(unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4f8402)
    #5 0x761459 in FileStream::copy() /home/user/xpdf-4.04/xpdf/Stream.cc:795:10
    #6 0x733fbb in Object::copy(Object*) /home/user/xpdf-4.04/xpdf/Object.cc:96:27
    #7 0x592bde in Object::arrayGet(int, Object*, int) /home/user/xpdf-4.04/xpdf/Object.h:243:19
    #8 0x592bde in Catalog::countPageTree(Object*) /home/user/xpdf-4.04/xpdf/Catalog.cc:566:12
    ...

SUMMARY: AddressSanitizer: stack-overflow (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e8a39) in __sanitizer::StackDepotBase<__sanitizer::StackDepotNode, 1, 20>::Put(__sanitizer::StackTrace, bool*)
==2685==ABORTING
```
# Environment
```
Ubuntu 20.04.1 LTS (docker)
clang-10
xpdf-4.04
```
# POC
[poc_Stream.cc](https://github.com/gandalf4a/crash_report/blob/main/xpdfreader/pdftotext/poc_Stream.cc.zip)

# Credit
Gandalf4a (QAXsrc of China)
