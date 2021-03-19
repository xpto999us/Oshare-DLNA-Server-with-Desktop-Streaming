Oshare Screen Server
This program is freeware, no warranties. Use it at your own risk.


This program is a modified version of Oshare Dlna Server, modified to stream desktop screen do smartv TVs (or other
dlna client), with low CPU and memory usage and hardware accelerated encoding.  Is intended to stream movies, solving incompatibilities
 issues with movie/smart TV. Some dlna servers permits on the fly transcoding for incompatible movies, but whithout reliable ff/rw
 (fast forwand/rewind), if a problem occur, movie must viewed form the beginning again. 
Playing movie on PC and stream desktop screen solve this problem, permits ff/rw, fine adjust of subtitles (font, size, colors), 
choose multiple PC video players, adjust image settings.
Video Players recommended: Smplayer, Potplayer or VLC.  Windows Media Player not recommended.

COMPATIBILITES/dlna clients running ok:
Android: Mezzmo, BubbleUPNP, Upnplay (all with external player VLC), Kodi, VLC, ArkMC 1.0 (arkmc avi mode only).
Windows:VLC, Kodi (kodi mpegts only), PowerDvd19 (powerdvd mpegts only).
TV:Samsung F8000 (AMD VCE hardware compression only with avi mode). 

1-INSTALL
-The program is portable, just run oShareUI.exe, but first install directshow filters (video an audio) to capture the desktop screen,
download and install ffmpeg.exe, and configure with "options" (radio.exe) or editing config.ini file.
A oshareui.exe restart is required when changing options. 

-If some dlls are required or missing, install Microsoft VC++ 2012 x86/x64 Runtime.

-Download ffmpeg.exe - static build- and copy to the same folder of the  oShareUI.exe.
ffmpeg.exe(download most recent STATIC build, static build is one executable only):https://www.gyan.dev/ffmpeg/builds/
(like https://www.gyan.dev/ffmpeg/builds/packages/ffmpeg-4.3.2-2021-02-02-full_build.7z, extract ffmpeg.exe and copy to oshareui.exe folder)
On 64 bit Windows, install 64 bit ffmpeg.exe to enable hardware acceleration.

-Install directshow audio and video capture filters.
For audio capture, install the included "screen capture recorder" (install audio and video directshow capture filters) or download 
last version from:
https://github.com/rdp/screen-capture-recorder-to-video-windows-free/releases
A directshow audio capture filter named "virtual-audio-capturer" will be installed.
(or configure stereo mix, see instructions at the end of this file).

-Included are two directshow video capture filters, others capture filters are listed at the end of this file:

-VCAM2 : gdi based, compatible com all Windows. Fixed capture rate (30fps), capture at
configured monitor resolution (no resize, optimized for performance). 32 bit color, no configuration files.

-VCAM3 :dxgi (desktop  duplication api 1.2) based, more fastest e better performance than gdi based version (vcam2) but only 
compatible with Windows 8 or above.
25 fps, 32 bit color. No resize, no configuration files. 

To install VCAM2 and VCAM3 filters (64 bit Windows):

-copy 32 bit Vcam2.dll and Vcam3.dll to Windows\Syswow64 folder.
-open a command prompt with administrator privileges, go to Windows\Syswow64 and register the dll:
regsvr32 vcam2.dll
regsvr32 vcam3.dll

-copy 64bit vcam2.dll and vcam3.dll to windows\system32 folder
-open a command prompt with admnistrator privileges, go to Windows\System32 and register the dll:
regsvr32 vcam2.dll
regsvr32 vcam3.dll

To unistall: go to folders above and unregister dlls with: 

regsvr32 -u vcam2.dll (or vcam3.dll)
and delete vcam2.dll and vcam3.dll

Both filters not have configuration or registry settings.


2)DEFAULT CONFIGURATION:
At first run, will be created a config.ini with default parameters, compatibles with most WiFi N networks.
H.264 video codec (libx264) with settings to 2-3 mbps video bit rate, AAC audio codec (CBR 192kbps),
no hardware acceleration, avi stream format, VCAM3 video capture filter, virtual-audio-capturer audio
capture filter.
This settings is compatible with most smart TVs and others dlna clients.

-Basic configuration is made by "radio.exe" program. 
-To ajust parameters, edit config.ini file. IMPORTANT: use only Notepad, Notepad++ or Notepad2,
do not use Word or other "graphical" editor. The first line is ffmpeg commands, its a long line, 
press "enter" only at end of line. Make a copy of edited config.ini, and don´t reconfigure
or settings will be lost. 

IMPORTANT: ffmpeg codec libx264 requires -pix_fmt yuv420p parameter or some TV will display msg
           "invalid file".  
           
3)RUNNING/STREAMING DESKTOP 
-run oshareui.exe, a "screenserver" icon must be visible (TV or other dlna client), select this video file, and 
the PC desktop screen will be send to TV. 

-------------------------------------
-Resize: resize increases cpu load, resize is made through ffmpeg commands or ajusting video capture filters
 (vcam2 and vcam3 not permit resize). To avoid increase CPU load, resize changing monitor resolution.  

-High CPU load: to reduce cpu load, adjust ffmpeg libx264 preset to:
 -preset ultrafast 

or change ffmpeg codec to mpeg4 and video bitrate 4mbps:
-c:v mpeg4 -b:v 4M  

or change video codec to mpeg2:
--c:v mpeg2video -b:v 5M -c:a mp2 -b:a 128K -f mpegts   

-Hardware acceleration: for Intel Quicksync and Nvidia NVENC check the respective ffmpeg commands
(https://trac.ffmpeg.org/wiki/HWAccelIntro), and alter the first line of config.ini (ffmpeg command line).

-For best performance uses Potplayer to play movies, check with end credits of a
movie with screen rolling effect. If possible, enable  DXVA2 decoding on player.

---------------------------------------- 
6)CONFIGURATION FILES:
oshare.ini: basic oshare configuration.
config.ini: ffmpeg configuration file, contains these lines:

FFMPEG="  " line command for ffmpeg.exe program, between "", this line can be edited for resize, hardware
acceleration or other commands. 

FORMAT=mpeg or avi stream format.   

HW=AMD or DISABLED (AMD VCE GPU acceleration for ffmpeg encoding).For Intel Quicksync amd Nvidia NVENC
edit the FFMPEG= line. Hardware encoders have less quality than libx264.  

BUFFER= internal read buffer, this is not the ffmpeg capture buffer. Adjust if stream has problems, like
TV showing msg "loading..." for long time. Default = 64000. Valid values 1000-100000.

VCAM2 (or VCAM3) Video Capture filters, for others video capture filters edit FFMPEG= line, check end
of this file for a list of directshow capture filters.
DXGI "EXPERIMENTAL" internal dxgi capture, maybe audio out of sync. 

16999999999  Size of the transmitted file (default 17GB).
Increase/decrease this value if dlna client (TV) report some error. 

--------------------------------
7)ERRORS AND TROUBLESHOOTING 

-To avoid loop, maximum number of ffmpeg.exe processes is limited to 6. 

-Oshareui.exe crash: run oshareui.exe again, if problem persist run oshreui.exe from system disk (c: or other disk with "Windows" folder in 
the same hdd/partition).
Check firewall and permissions, oshare read and write config files.    


If image is distorted include -pix_fmt yuv420p in ffmpeg command line.

If audio is distorted or no audio, disable audio enhancements in Windows Control Panel (hardware and sounds), restart Windows.

-If no image is displayed -  TV:

a)delete config.ini and run oShareUI.exe. Check if config.ini file exists. Run VLC player (same PC), click Universal Plug n Play option.
 If no image is displayed on VLC, check ffmpeg commands.
-open a command prompt on the same folder of oShareUI.exe and run:

ffmpeg -f dshow -i video="Virtual Cam2:audio=virtual-audio-capturer"  -c:v libx264 -preset fast -crf 27 output.mp4

for test video capture only:
ffmpeg -f dshow -i video="Virtual Cam2" output.mp4

for test audio capture only:
ffmpeg  -f dshow -i audio="virtual-audio-capturer" output.mp3


Wait 20 ou 30 seconds and press ctrl break.

play output.mp4 in VLC player, if no captured image is displayed check installed directshow filters:

ffmpeg -list_devices true -f dshow -i dummy

check if Virtual Cam3 (or other filter configured in the ffmpeg command line of config.ini file) 
is in the list displayed. If not, reinstall directshow filter. The name of the filter displayed must be identical of ffmpeg command 
line of config.ini file, including special characters.

IMPORTANT: 64 bit ffmpeg uses 64 bit directshow filters, 32 bit ffmpeg uses 32 bit directshow filters. 

c)No "Oshare" icon displayed:

-oshareui.exe->"general"->"save" to restart  server.


d)Programs to check directshow filters (diagnostic and register/unregister filters):

DsfMftViewer http://bluesky23.yukishigure.com/en/DsfMftViewer.html
Filmerit  http://paul.glagla.free.fr/filmerit_en.htm
Sherlock http://www.updatexp.com/sherlock-codec-detective.html
InstalledCodec  https://www.nirsoft.net/utils/installed_codec.html
 
-----------------------------------------------
4)LIST OF DIRECTSHOW FILTERS:

For 64bit FFMPEG 64 bit direcshow filters are required. For 32 bit FFMPEG, 32 bits directshow filters. 

http://betterlogic.com/roger/2010/07/list-of-available-directshow-screen-capture-filters/

To configure some directshow filters, virtualdub2 can be used (File-->capture avi-->
Device-->(directshow filter) Video-->"capture filter" option).

-Audio capture with "Stereo Mix":
-Configuring "Stereo Mix" audio filter:Go down to the audio icon in your system tray, right-click it, and go to “Recording Devices”
 to open up the proper settings pane.In the pane, right-click on a blank area, and make sure both “View Disabled Devices” and
 “View Disconnected Devices” options are checked.  You should see a “Stereo Mix” option appear.In the pane, right-click on a blank area,
 and make sure both “View Disabled Devices” and “View Disconnected Devices” options are checked.  You should see a “Stereo Mix” option appear.
Right-click on “Stereo Mix” and click “Enable” to be able to use it.

-http://www.umediaserver.net/components/index.html 
Configure (registry settings):
HKLM\SOFTWARE\UNREAL\Live\UScreenCapture
DWORD: MonitorNum
DWORD: Left
DWORD: Right
DWORD: Top
DWORD: Bottom
DWORD: FrameRate
DWORD: ShowCursor
DWORD: CaptureLayeredWindows
MonitorNum            - is monitor to capture. Four monitors are supported. Value of 0 means capture from all monitors.
Left                  - is the x-coordinate of the upper left corner of the region to capture in pixels.
Right                 - is the x-coordinate of the lower right corner of the region to capture in pixels.
Top                   - is the y-coordinate of the upper left corner of the region to capture in pixels.
Bottom                - is the y-coordinate of the lower right corner of the region to capture in pixels.
FrameRate             - the valid range is 1 to 60.
ShowCursor            - 1 to capture cursor, 0 not to capture.
CaptureLayeredWindows - 1 to capture layered windows, 0 not to capture.

1280x720:
Bottom:720
Right:1280

-https://github.com/rdp/screen-capture-recorder-to-video-windows-free
(dxgi version):https://download.csdn.net/download/xjb2006/12712767

-http://mosax.sakura.ne.jp/yp4g/fswiki.cgi?action=ATTACH&page=SCFH+DSF&file=SCFHDSF041%2Ezip
https://koitsu.wordpress.com/2009/09/12/how-to-install-and-use-scfh-dsf/
(to configure:HKEY_CURRENT_USER/Software/SCFH DSF/).

-Dxtory(Directx capture filter included, to stream movies a directx video player is required, like mpv.exe or Smplayer). 
Some manual actions are required to start/stop capture. (not free).

-https://bluesky23.yukishigure.com/en/BlueskyVideoCapture.html (not free).

-VCAM2 : no config, fixed frame rate 30 fps (CFR), captures at configured monitor resolution.
         Compatible with most programs (Virtualdub, Mezzmo, FFMPEG).Windows all. 
-VCAM3 : no config, fixed frame rate of 25 fps. Requires Windows 8+ (DXGI Desktop Duplication Api 1.2).
         Best option to capture.
         Compatible with FFMPEG, VirtualDub, Mezzmo and some other programs. 
=============================================================


 

        