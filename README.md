# crash_report
<table>
    <tr>
        <td>产品</td>
        <td>程序</td>
        <td>版本</td>
        <td>源码</td>
        <td>位置</td>
        <td>函数</td>
        <td>类型</td>
        <td>数量</td>
        <td>URL</td>
        <td>时间</td>
    </tr>
    <tr>
        <td>libpng</td>
        <td>pngimage</td>
        <td>v1.6.39</td>
        <td>https://github.com/glennrp/libpng</td>
        <td>./contrib/libtests/pngimage.c:1249</td>
        <td>compare_read()</td>
        <td>heap-buffer-overflow</td>
        <td>1</td>
        <td>https://github.com/glennrp/libpng/issues/481</td>
        <td>2023.6.14</td>
    </tr>
    <tr>
        <td rowspan="10">libtiff</td>
        <td rowspan="10">liffcrop</td>
        <td rowspan="10">4.5.0</td>
        <td rowspan="10">https://gitlab.com/libtiff/libtiff</td>
        <td>./libtiff/tif_unix.c:345</td>
        <td>_TIFFmemcpy()</td>
        <td>heap-buffer-overflow & heap-use-after-free & SIGSEGV</td>
        <td>1</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/573</td>
        <td rowspan="10">2023.5.11</td>        
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3773</td>
        <td rowspan="2">extractContigSamplesShifted8bits()</td>
        <td rowspan="9">heap-buffer-overflow</td>
        <td rowspan="2">2</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/563</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3760</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/562</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3877</td>
        <td rowspan="2">extractContigSamplesShifted16bits()</td>
        <td rowspan="2">2</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/561</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3863</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/564</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3982</td>
        <td rowspan="3">extractContigSamplesShifted24bits()</td>
        <td rowspan="3">3</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/565</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3984</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/566</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3998</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/567</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:4106</td>
        <td rowspan="2">extractContigSamplesShifted32bits()</td>
        <td rowspan="2">5</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/568</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:4108,4110,4112,4124</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/569</td>
    </tr>
    <tr>
        <td rowspan="5">xpdfreader</td>
        <td rowspan="5">pdftotext</td>
        <td rowspan="5">4.04</td>
        <td rowspan="5">https://dl.xpdfreader.com/xpdf-latest.tar.gz</td>
        <td>./goo/GString.cc:119</td>
        <td>GString::resize(int)</td>
        <td rowspan="4">stack-overflow</td>
        <td>1</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42378</td>
        <td rowspan="5">2022.12.26</td>
    </tr>
    </tr>
        <td>./xpdf/Stream.cc:795</td>
        <td>FileStream::copy()</td>
        <td>1</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42376</td>
    </tr>
    </tr>
        <td>./xpdf/Object.cc:81</td>
        <td>Object::copy(Object*)</td>
        <td>1</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42377</td>
    </tr>
    </tr>
        <td>./goo/gmem.cc:148</td>
        <td>gmalloc(int)</td>
        <td>1</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42379</td>
    </tr>
    <tr>
        <td>./xpdf/TextOutputDev.cc:988</td>
        <td>TextLine::TextLine()</td>
        <td rowspan="1">SIGSEGV</td>
        <td>1</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=44307</td>
    </tr>
</table>
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
