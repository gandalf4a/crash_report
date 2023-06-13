# issue 
https://forum.xpdfreader.com/viewtopic.php?p=44307
# Summary
Hi there, I use my fuzzer for fuzzing the binary pdftotext, the version of XPDF is the latest (v4.04) and the operation system is Ubuntu 20.04.1 LTS and this binary crash with the following.

# Details
```
CODE: SELECT ALL

id:000032,sig:11,src:005277+000452,time:40632165,execs:9437796,op:splice,rep:8
AddressSanitizer:DEADLYSIGNAL
=================================================================
==2848==ERROR: AddressSanitizer: SEGV on unknown address (pc 0x000000500e9d bp 0x608000004420 sp 0x7fff05eebba0 T0)
==2848==The signal is caused by a READ memory access.
==2848==Hint: this fault was caused by a dereference of a high value address (see register values below).  Dissassemble the provided pc to learn which register was used.
    #0 0x500e9d in TextLine::TextLine(GList*, double, double, double, double, double) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:988:16
    #1 0x541136 in TextPage::buildLine(GList*, int, double, double, double, double) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:5162:14
    #2 0x53ef3f in TextPage::buildLine(TextBlock*) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:5084:20
    #3 0x53e791 in TextPage::buildLines(TextBlock*, GList*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4884:12
    #4 0x53e6bb in TextPage::buildLines(TextBlock*, GList*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4892:7
    #5 0x53bf8f in TextPage::buildColumn(TextBlock*) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4708:3
    #6 0x53be85 in TextPage::buildColumns2(TextBlock*, GList*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4678:11
    #7 0x53bdcb in TextPage::buildColumns2(TextBlock*, GList*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4690:2
    #8 0x50a1c4 in TextPage::buildColumns(TextBlock*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:4666:3
    #9 0x50a1c4 in TextPage::writeReadingOrder(void*, void (*)(void*, char const*, int), UnicodeMap*, char*, int, char*, int) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:1754:13
    #10 0x509be8 in TextPage::write(void*, void (*)(void*, char const*, int)) /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:1686:5
    #11 0x5fe123 in Gfx::~Gfx() /home/user/xpdf-4.04/xpdf/Gfx.cc:618:10
    #12 0x74476b in Page::displaySlice(OutputDev*, double, double, int, int, int, int, int, int, int, int, int (*)(void*), void*) /home/user/xpdf-4.04/xpdf/Page.cc:455:3
    #13 0x743be1 in Page::display(OutputDev*, double, double, int, int, int, int, int (*)(void*), void*) /home/user/xpdf-4.04/xpdf/Page.cc:368:3
    #14 0x7530fe in PDFDoc::displayPage(OutputDev*, int, double, double, int, int, int, int, int (*)(void*), void*) /home/user/xpdf-4.04/xpdf/PDFDoc.cc:442:27
    #15 0x7530fe in PDFDoc::displayPages(OutputDev*, int, int, double, double, int, int, int, int, int (*)(void*), void*) /home/user/xpdf-4.04/xpdf/PDFDoc.cc:460:5
    #16 0x554d5f in main /home/user/xpdf-4.04/xpdf/pdftotext.cc:306:10
    #17 0x7f41e82bc082 in __libc_start_main /build/glibc-SzIz7B/glibc-2.31/csu/../csu/libc-start.c:308:16
    #18 0x44e68d in _start (/home/user/xpdf-4.04/xpdf/pdftotext+0x44e68d)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/user/xpdf-4.04/xpdf/TextOutputDev.cc:988:16 in TextLine::TextLine(GList*, double, double, double, double, double)
==2848==ABORTING
```
# Environment
```
Ubuntu 20.04.1 LTS (docker)
clang-10
xpdf-4.04
```
# POC
[poc_SEGV](https://github.com/gandalf4a/crash_report/blob/main/xpdfreader/pdftotext/poc_SEGV.zip)
# Credit
Gandalf4a (QAXsrc of China)
