# How to debug an android apk without source code
## Objectives:

1. setup env to debug an android apk without source code
2. able to set breakpoints

## Toolkit:

2. android studio (v3.4.1)
3. [jadx](https://github.com/skylot/jadx) -> to convert apk to java
4. kingroot -> to gain root privilege on devices
5. [xposed](https://repo.xposed.info/) -> backbone of android reverse engineering
6. [jdk1.8u151](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html) -> for legacy Dalvik Debug Monitor Service (ddms)

## Steps(OS X):

1. install android studio, android sdk
2. Install jdk1.8u151 for ddms, [why 8u151](https://stackoverflow.com/questions/47089757/android-device-monitor-freezes-on-mac-os-x), [what if it does not work](https://gist.github.com/sshnakamoto/074b5428a25b467e4402d97ded02be9d)
3. open the target apk with **Profile or Debug Apk**, android studio will convert dex byte code to [smali](https://github.com/JesusFreke/smali), & auto generate a project in **ApkProjects** in which ever your android studio setup path is
4. unpack target apk using **jadx or jadx-gui**, there will be a dir named "sources", ln/mv/cp this dir to the project inside **ApkProjects**, android studio will auto-index them
5. open **File -> Project Structure -> Modules**, find the project & locate the **Sources** panel, in the tree structure, mark the java source dir in step 5 as **"Sources"**, now u will have both smali code & java code ready for debugging
6. gain root privilege on target device
7. install xposed, & install a module named: **InstallerOpt**, activate this module with **Debug Applications** ON, to allow android studio to debug any apk without manipulating the manifest.xml
8. happy ~~or not~~ debugging 

## Gotchas:

1. changing the AndroidManifest.xml to make the apk debuggable probably will NOT work for most commercial apks, that's where xposed & InstallerOpt comes in
2. although jadx could convert dex byte code into java, the code will still remain obfuscated if the origin apk was released that way
3. a single java class could be scattered into several smali files
4. android studio already has everything u need to debug an apk, although for devices with sdk version <  26, the newer **profiler** won't work, that's why we need legacy ddms located in android sdk dir -> tools
5. if u are not able to launch ddms, read **Step 2**, carefully
6. most released apks probably would ban emulators, good luck with that