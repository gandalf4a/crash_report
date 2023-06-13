# crash_report

|  产品   | 程序文件  | 代码文件| 函数| 漏洞类型|漏洞数量| url|
|  ----  | ----  | ----  | ----  | ----  |----|----|
| libpng  | pngimage |pngimage.c|compare_read|heap-buffer-overflow|1|https://github.com/glennrp/libpng/issues/481|
|libtiff|liffcrop|tif_unix.c|TIFFmemcpy|heap-buffer-overflow & heap-use-after-free &  SIGSEGV|1|https://gitlab.com/libtiff/libtiff/-/issues/573|
|||tiffcrop.c|extractContigSamplesShifted8bits|2|https://gitlab.com/libtiff/libtiff/-/issues/563|
||||||https://gitlab.com/libtiff/libtiff/-/issues/562|

## libpng
### pngimage
##### 1_heap-buffer-overflow_compare_read
https://github.com/glennrp/libpng/issues/481

## libtiff
### liffcrop 
####  tif_unix.c_TIFFmemcpy
##### heap-buffer-overflow & heap-use-after-free &  SIGSEGV
https://gitlab.com/libtiff/libtiff/-/issues/573
#### tiffcrop.c
##### 2_heap-buffer-overflow_extractContigSamplesShifted8bits
https://gitlab.com/libtiff/libtiff/-/issues/563

https://gitlab.com/libtiff/libtiff/-/issues/562
##### 2_heap-buffer-overflow_extractContigSamplesShifted16bits
https://gitlab.com/libtiff/libtiff/-/issues/561

https://gitlab.com/libtiff/libtiff/-/issues/564

##### 3_heap-buffer-overflow_extractContigSamplesShifted24bits
https://gitlab.com/libtiff/libtiff/-/issues/565

https://gitlab.com/libtiff/libtiff/-/issues/566

https://gitlab.com/libtiff/libtiff/-/issues/567
##### 5_heap-buffer-overflow_extractContigSamplesShifted32bits
https://gitlab.com/libtiff/libtiff/-/issues/568

https://gitlab.com/libtiff/libtiff/-/issues/569

## xpdfreader
### pdftotext
##### stack-overflow_GString.cc
https://forum.xpdfreader.com/viewtopic.php?t=42378
##### stack-overflow_Object.cc
https://forum.xpdfreader.com/viewtopic.php?t=42376
##### stack-overflow_Stream.cc
https://forum.xpdfreader.com/viewtopic.php?t=42377
##### stack-overflow_gmem.cc
https://forum.xpdfreader.com/viewtopic.php?t=42379
##### SIGSEGV_TextOutputDev.cc
https://forum.xpdfreader.com/viewtopic.php?p=44307
