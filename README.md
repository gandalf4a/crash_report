# crash & poc
<table>
    <tr>
        <td>project</td>
        <td>tool</td>
        <td>version</td>
        <td>source</td>
        <td>path</td>
        <td>function</td>
        <td>type</td>
        <td>issue</td>
        <td>time</td>
    </tr>
    <tr>
        <td>JavaScript</td>
        <td>jerry</td>
        <td>commit a588e4966175a190ec6350b2a3689d30ed017ec9 (HEAD -> master, origin/master, origin/HEAD)</td>
        <td>https://github.com/jerryscript-project/jerryscript</td>
        <td> </td>
        <td> </td>
        <td>SEGV</td>
        <td>https://github.com/jerryscript-project/jerryscript/issues/5101</td>
        <td>2023.10.4</td>
    </tr>
    <tr>
        <td rowspan="2">Mozilla</td>
        <td rowspan="2">Spidermonkey</td>
        <td rowspan="2">commit b0d28aecd58cbd2db00974db2ef8456856169fb4 (HEAD -> master, origin/master, origin/HEAD)</td>
        <td rowspan="2">https://github.com/mozilla/gecko-dev</td>
        <td rowspan="2"> </td>
        <td rowspan="2"> </td>
        <td rowspan="2">SEGV</td>
        <td>https://bugzilla.mozilla.org/show_bug.cgi?id=1856649</td>
        <td rowspan="2">2023.10.3</td>
    </tr>
    <tr>
        <td>https://bugzilla.mozilla.org/show_bug.cgi?id=1856646</td>
    </tr>
    <tr>
        <td>Webkit</td>
        <td>JavaScriptCore</td>
        <td>commit 1242f2ee324a89ec535c86d2fe89a86b0e8a1e52 (HEAD -> main, origin/main, origin/HEAD)</td>
        <td>https://github.com/WebKit/WebKit.git</td>
        <td> </td>
        <td> </td>
        <td>memory leaks</td>
        <td>https://bugs.webkit.org/show_bug.cgi?id=262370</td>
        <td>2023.9.29</td>
    </tr>
    <tr>
        <td>libpng</td>
        <td>pngimage</td>
        <td>v1.6.39</td>
        <td>https://github.com/glennrp/libpng</td>
        <td>./contrib/libtests/pngimage.c:1249</td>
        <td>compare_read()</td>
        <td>heap-buffer-overflow</td>
        <td>https://github.com/glennrp/libpng/issues/481</td>
        <td>2023.6.14</td>
    </tr>
    <tr>
        <td rowspan="10">libtiff</td>
        <td rowspan="10">tiffcrop</td>
        <td rowspan="10">4.5.0</td>
        <td rowspan="10">https://gitlab.com/libtiff/libtiff</td>
        <td>./libtiff/tif_unix.c:345</td>
        <td>_TIFFmemcpy()</td>
        <td>heap-buffer-overflow & heap-use-after-free & SIGSEGV</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/573</td>
        <td rowspan="10">2023.5.11</td>        
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3773</td>
        <td rowspan="2">extractContigSamplesShifted8bits()</td>
        <td rowspan="9">heap-buffer-overflow</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/563</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3760</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/562</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3877</td>
        <td rowspan="2">extractContigSamplesShifted16bits()</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/561</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3863</td>
        <td>https://gitlab.com/libtiff/libtiff/-/issues/564</td>
    </tr>
    <tr>
        <td>./tools/tiffcrop.c:3982</td>
        <td rowspan="3">extractContigSamplesShifted24bits()</td>
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
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42378</td>
        <td rowspan="5">2022.12.26</td>
    </tr>
    </tr>
        <td>./xpdf/Stream.cc:795</td>
        <td>FileStream::copy()</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42376</td>
    </tr>
    </tr>
        <td>./xpdf/Object.cc:81</td>
        <td>Object::copy(Object*)</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42377</td>
    </tr>
    </tr>
        <td>./goo/gmem.cc:148</td>
        <td>gmalloc(int)</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=42379</td>
    </tr>
    <tr>
        <td>./xpdf/TextOutputDev.cc:988</td>
        <td>TextLine::TextLine()</td>
        <td>SIGSEGV</td>
        <td>https://forum.xpdfreader.com/viewtopic.php?t=44307</td>
    </tr>
</table>

