# SEGV in vlc in avformat_OpenDemux /home/user/fuzzing_vlc/vlc-3.0.19/modules/demux/avformat/demux.c:323:13

# Version
```
$ wget https://download.videolan.org/pub/videolan/vlc/3.0.19/vlc-3.0.19.tar.xz
$ ./vlc-static -v
VLC media player 3.0.19 Vetinari (revision 3.0.19-0-g32b50de2a28)
```

# Platform
```
$ uname -a
Linux user-AYA-NEO-FOUNDER 5.19.0-43-generic #44~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Mon May 22 13:39:36 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

# Asan
```
VLC media player 3.0.19 Vetinari (revision 3.0.19-0-g32b50de2a28)
[000061200004af80] main interface error: no suitable interface module
[0000613000000100] main libvlc error: interface "globalhotkeys,none" initialization failed
[0000613000000100] main libvlc: Running vlc with the default interface. Use 'cvlc' to use vlc without interface.
Remote control interface initialized. Type `help' for help.
/home/user/fuzzing_vlc/out/default/crashes/id:000001,sig:11,src:000927,time:139529,execs:17675,op:flip1,pos:61:3: namespace error : xmlns: '5rn:mpeg:dash:schema:mpd:2011' is not a valid URI
<MPD xmlns="5rn:mpeg:dash:schema:mpd:2011" minBufferTime="PT1.000S" type="dynami
                                          ^
AddressSanitizer:DEADLYSIGNAL
=================================================================
==2297874==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x7f1b8a01b430 bp 0x60c00000c280 sp 0x7f1b82a47f80 T9)
==2297874==The signal is caused by a READ memory access.
==2297874==Hint: address points to the zero page.
    #0 0x7f1b8a01b430 in av_strcasecmp (/lib/x86_64-linux-gnu/libavutil.so.56+0x1b430)
    #1 0x7f1b8ba86f8e  (/lib/x86_64-linux-gnu/libavformat.so.58+0x86f8e)
    #2 0x7f1b8ba872c0  (/lib/x86_64-linux-gnu/libavformat.so.58+0x872c0)
    #3 0x7f1b8bbcdf9f in avformat_open_input (/lib/x86_64-linux-gnu/libavformat.so.58+0x1cdf9f)
    #4 0x7f1b8c50a261 in avformat_OpenDemux /home/user/fuzzing_vlc/vlc-3.0.19/modules/demux/avformat/demux.c:323:13
    #5 0x7f1ba2cd1366 in module_load /home/user/fuzzing_vlc/vlc-3.0.19/src/modules/modules.c:183:15
    #6 0x7f1ba2cd1366 in vlc_module_load /home/user/fuzzing_vlc/vlc-3.0.19/src/modules/modules.c:280:23
    #7 0x7f1ba2d321dc in demux_NewAdvanced /home/user/fuzzing_vlc/vlc-3.0.19/src/input/demux.c:264:29
    #8 0x7f1ba2d81455 in InputDemuxNew /home/user/fuzzing_vlc/vlc-3.0.19/src/input/input.c:2644:15
    #9 0x7f1ba2d81455 in InputSourceNew /home/user/fuzzing_vlc/vlc-3.0.19/src/input/input.c:2754:19
    #10 0x7f1ba2d6914e in Init /home/user/fuzzing_vlc/vlc-3.0.19/src/input/input.c:1378:14
    #11 0x7f1ba2d74847 in Run /home/user/fuzzing_vlc/vlc-3.0.19/src/input/input.c:497:10
    #12 0x7f1ba2894ac2 in start_thread nptl/./nptl/pthread_create.c:442:8
    #13 0x7f1ba2926a3f  misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:81

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV (/lib/x86_64-linux-gnu/libavutil.so.56+0x1b430) in av_strcasecmp
Thread T9 created by T2 here:
    #0 0x485a9c in pthread_create (/home/user/fuzzing_vlc/vlc-3.0.19/bin/vlc-static+0x485a9c)
    #1 0x7f1ba2e766d1 in vlc_clone_attr /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/thread.c:482:11
    #2 0x7f1ba2e7633f in vlc_clone /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/thread.c:494:12
    #3 0x7f1ba2d7468a in input_Start /home/user/fuzzing_vlc/vlc-3.0.19/src/input/input.c:183:25
    #4 0x7f1ba2894ac2 in start_thread nptl/./nptl/pthread_create.c:442:8

Thread T2 created by T0 here:
    #0 0x485a9c in pthread_create (/home/user/fuzzing_vlc/vlc-3.0.19/bin/vlc-static+0x485a9c)
    #1 0x7f1ba2e766d1 in vlc_clone_attr /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/thread.c:482:11
    #2 0x7f1ba2e7633f in vlc_clone /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/thread.c:494:12
    #3 0x7f1ba2cec10b in playlist_Activate /home/user/fuzzing_vlc/vlc-3.0.19/src/playlist/thread.c:55:9
    #4 0x7f1ba2ce9437 in intf_GetPlaylist /home/user/fuzzing_vlc/vlc-3.0.19/src/interface/interface.c:145:20
    #5 0x7f1ba2ce9437 in libvlc_InternalAddIntf /home/user/fuzzing_vlc/vlc-3.0.19/src/interface/interface.c:203:28
    #6 0x7f1ba2e74064 in system_ConfigureDbus /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/specific.c:52:9
    #7 0x7f1ba2e74064 in system_Configure /home/user/fuzzing_vlc/vlc-3.0.19/src/posix/specific.c:160:5
    #8 0x7f1ba2ca1633 in libvlc_InternalInit /home/user/fuzzing_vlc/vlc-3.0.19/src/libvlc.c:284:5
    #9 0x7f1ba306903b in libvlc_new /home/user/fuzzing_vlc/vlc-3.0.19/lib/core.c:59:9
    #10 0x4cc86e in main /home/user/fuzzing_vlc/vlc-3.0.19/bin/vlc.c:231:30
    #11 0x7f1ba2829d8f in __libc_start_call_main csu/../sysdeps/nptl/libc_start_call_main.h:58:16

==2297874==ABORTING
```

# Reproduce
```
./bin/vlc-static poc
```
 
# POC File
https://github.com/gandalf4a/crash_report/blob/main/vlc/vlc_segv

# Credit
```
Gandalf4a
```
