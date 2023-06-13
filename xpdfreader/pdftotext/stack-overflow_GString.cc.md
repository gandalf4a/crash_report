# issue
https://forum.xpdfreader.com/viewtopic.php?t=42378

# Summary
Hi there, I use my fuzzer for fuzzing the binary pdftotext, the version of XPDF is the latest (v4.04) and the operation system is Ubuntu 20.04.1 LTS and this binary crash with the following.

# Details
```
CODE: SELECT ALL

id:000058,sig:11,src:002481,time:170693918,execs:40892220,op:havoc,rep:16

AddressSanitizer:DEADLYSIGNAL
=================================================================
==2713==ERROR: AddressSanitizer: stack-overflow on address 0x7ffe09171fdc (pc 0x0000004e0281 bp 0x000000010000 sp 0x7ffe09171fd0 T0)
    #0 0x4e0281 in __sanitizer::MmapFixedImpl(unsigned long, unsigned long, bool, char const*) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e0281)
    #1 0x454cb0 in __sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> >::PopulateFreeArray(__sanitizer::AllocatorStats*, unsigned long, __sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> >::RegionInfo*, unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x454cb0)
    #2 0x454b83 in __sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> >::GetFromAllocator(__sanitizer::AllocatorStats*, unsigned long, unsigned int*, unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x454b83)
    #3 0x454892 in __sanitizer::SizeClassAllocator64LocalCache<__sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> > >::Refill(__sanitizer::SizeClassAllocator64LocalCache<__sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> > >::PerClass*, __sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> >*, unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x454892)
    #4 0x45450e in __sanitizer::CombinedAllocator<__sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> >, __sanitizer::LargeMmapAllocatorPtrArrayDynamic>::Allocate(__sanitizer::SizeClassAllocator64LocalCache<__sanitizer::SizeClassAllocator64<__asan::AP64<__sanitizer::LocalAddressSpaceView> > >*, unsigned long, unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x45450e)
    #5 0x44fba3 in __asan::Allocator::Allocate(unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType, bool) (/home/user/xpdf-4.04/xpdf/pdftotext+0x44fba3)
    #6 0x450749 in __asan::asan_memalign(unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType) (/home/user/xpdf-4.04/xpdf/pdftotext+0x450749)
    #7 0x4f8512 in operator new[](unsigned long) (/home/user/xpdf-4.04/xpdf/pdftotext+0x4f8512)
    #8 0x7ee996 in GString::resize(int) /home/user/xpdf-4.04/goo/GString.cc:119:9
    #9 0x7eef9c in GString::GString(GString*) /home/user/xpdf-4.04/goo/GString.cc:163:3
    #10 0x733dc1 in GString::copy() /home/user/xpdf-4.04/goo/GString.h:42:32
    #11 0x733dc1 in Object::copy(Object*) /home/user/xpdf-4.04/xpdf/Object.cc:84:27
    #12 0x592bde in Object::arrayGet(int, Object*, int) /home/user/xpdf-4.04/xpdf/Object.h:243:19
    #13 0x592bde in Catalog::countPageTree(Object*) /home/user/xpdf-4.04/xpdf/Catalog.cc:566:12
    ...

SUMMARY: AddressSanitizer: stack-overflow (/home/user/xpdf-4.04/xpdf/pdftotext+0x4e0281) in __sanitizer::MmapFixedImpl(unsigned long, unsigned long, bool, char const*)
==2713==ABORTING
```
# Environment
```
Ubuntu 20.04.1 LTS (docker)
clang-10
xpdf-4.04
```
# POC
[poc_GString.cc](https://github.com/gandalf4a/crash_report/blob/main/xpdfreader/pdftotext/poc_GString.cc.zip)
# Credit
Gandalf4a (QAXsrc of China)
