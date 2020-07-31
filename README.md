# notes

## ios debug devices
https://www.jianshu.com/p/8c99cb119bb3

## Upgrade brew with mirrors
Install: https://gitee.com/simdsoft/brew-install  
Update: modify /usr/local/Homebrew/Library/Homebrew/install.sh  
change homebrew-core repo to https://gitee.com/simdsoft/homebrew-core  
https://www.jianshu.com/p/62f1b963baa6  

## Dump cmake builtin macros
```cmake
get_cmake_property(_variableNames VARIABLES)
foreach (_variableName ${_variableNames})
  message(STATUS "${_variableName}=${${_variableName}}")
endforeach()
```

## Get AAssetManager by ndk
https://stackoverflow.com/questions/58980171/using-aassetmanager-fromjava-within-plugin-not-directly-called-from-java-vm-cal

## Gets expand/collapse icon size of TreeView
```cpp
auto themeData = OpenThemeData(m_wndTreeView.GetSafeHwnd(), VSCLASS_TREEVIEW);
SIZE tvButtonSize = { 0 };
GetThemePartSize(themeData, NULL, TVP_GLYPH, GLPS_OPENED, NULL, TS_DRAW, &tvButtonSize);
CloseThemeData(themeData);
```

## vs2019 cl.exe optimize options
https://docs.microsoft.com/en-us/cpp/build/reference/o1-o2-minimize-size-maximize-speed?view=vs-2019

## Apple cert profile cache directory
```~/Library/MobileDevice/Provisioning Profiles```

## Create Visual Studio 2019 offline installer
For C++ desktop development, run:
```bat
vs_community.exe --layout D:\vs16-offline --add Microsoft.VisualStudio.Workload.NativeDesktop --includeRecommended --lang en-US
```
For .Net & C++ desktop development, run:
```bat
vs_community.exe --layout D:\vs16-offline --add Microsoft.VisualStudio.Workload.NativeDesktop --add Microsoft.VisualStudio.Workload.ManagedDesktop --includeRecommended  --lang en-US
```

## build apk, config noCompress file type at app/build.gradle
```gradle
android {
    aaptOptions {
       noCompress 'mp3','ogg','wav','mp4','ttf'
    }
}
```

## wampserver 3.x
### Access form other computer on the local area network
1. Change http.conf ```Require local``` to ```Require all granted```
1. Change extra/http-vhosts.conf ```Require local``` to ```Require all granted```
2. Check firewall strategy, ensure the httpd.exe in allow list.

## Build openal-soft dependencies for android platform
### OS: Ubuntu 18.04.3 or install it with VMWare

### install dep-tools
```sh
apt-get update
apt-get install gcc-multilib
apt-get install autoconf automake cmake libtool git
```

### make-standalone-toolchain
prepare: ```cp /usr/bin/python3.6 /usr/bin/python```  
```sh
"$ANDROID_NDK/build/tools/make-standalone-toolchain.sh" --arch="arm" --platform="14" --stl=gnustl --install-dir="/opt/arm-standalone-toolchain" --verbose
"$ANDROID_NDK/build/tools/make-standalone-toolchain.sh" --arch="arm64" --platform="21" --stl=gnustl --install-dir="/opt/arm64-standalone-toolchain"  --verbose
```

### Add standalone toolchain's bin directory to PATH
```sh
export PATH=/opt/arm-standalone-toolchain/bin:$PATH  
export PATH=/opt/arm64-standalone-toolchain/bin:$PATH
```

### openal-soft  
```sh
git clone https://github.com/halx99/openal-soft
git checkout v1.19-aspec
export CMAKE_FIND_ROOT_PATH=/opt/arm-standalone-toolchain
export CMAKE_FIND_ROOT_PATH=/opt/arm64-standalone-toolchain
cd build
# if api-level is 9, remove -DHAVE_LOG2F=1
cmake .. -DCMAKE_TOOLCHAIN_FILE=../XCompile-Android.txt -DHOST=arm-linux-androideabi -DCMAKE_C_FLAGS="-mfloat-abi=softfp -mfpu=neon" -DCMAKE_BUILD_TYPE=Release -DHAVE_LOG2F=1
cmake .. -DCMAKE_TOOLCHAIN_FILE=../XCompile-Android.txt -DHOST=aarch64-linux-android -DCMAKE_BUILD_TYPE=Release
```  

CMAKE_BUILD_TYPE options: Debug Release RelWithDebInfo MinSizeRel

### mpg123
download: http://www.mpg123.de/  
```sh
curl http://www.mpg123.de/download/mpg123-1.25.13.tar.bz2 --output ./mpg123-1.25.13.tar.bz2
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc --with-cpu=neon
./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc --with-cpu=neon64
```  
for non-win32 platforms, you should copy mpg123.h from ```src/libmpg123/mpg123.h``` after configure finished.

### libogg-1.3.3:
```sh
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc
./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc
```
### libogg-1.3.4 (require android-cmake: https://github.com/taka-no-me/android-cmake)
```sh
mkdir build_arm64
cmake .. -DCMAKE_TOOLCHAIN_FILE=../../android-cmake/android.toolchain.cmake -DANDROID_NDK=/usr/local/src/android-ndk-r14b -DCMAKE_BUILD_TYPE=Release -DANDROID_ABI="arm64-v8a" ANDROID_NATIVE_API_LEVEL=android-21 .. -DCMAKE_MAKE_PROGRAM=/opt/arm64-standalone-toolchain/bin/makecmake -DCMAKE_TOOLCHAIN_FILE=../../android-cmake/android.toolchain.cmake -DANDROID_NDK=/usr/local/src/android-ndk-r14b -DCMAKE_BUILD_TYPE=Release -DANDROID_ABI="arm64-v8a" -DCMAKE_MAKE_PROGRAM=/usr/local/src/android-ndk-r14b/prebuilt/linux-x86_64/bin/make
```

### libvorbis-1.3.6 with libogg-1.3.3: 
```sh
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.3/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.3/src/.libs/
./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.3/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.3/src/.libs/
```

### libvorbis-1.3.6 with libogg-1.3.4: 
```sh
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.4/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.4/src/.libs/
./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.4/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.4/build_arm64
```

### strip symbols
remove static library debug symbol: ```arm-linux-androideabi-strip -g -S -d --strip-debug libogg.a -o libogg.a.s```  
remove dynamic library debug symbol: ```arm-linux-androideabi-strip libopenal.so -o libopenal.so.s```  
  
arm64-v8a: use **aarch64-linux-android-strip**


## readthedocs dev
1. Install [Python-3.7.3](https://www.python.org/ftp/python/3.7.3/python-3.7.3-amd64.exe)
2. Install Python Extensions: sphinx, sphinx_rtd_theme, sphinx_tabs: ```pip install sphinx sphinx_rtd_theme sphinx_tabs``` for vscode rst preview.
For more detail, see: https://docs.restructuredtext.net/articles/prerequisites.html
3. Install [Microsoft VS Code](https://code.visualstudio.com/)
4. Install Microsoft VS Code extension: reStructedText, just search ```rst```

## Use google angle GLES-2.0 in cocos2d-x
1. Build angle latest source from google
2. Copy libGLESv2.dll, libEGL.dll to your game directory
3. Use glad to generate with OpenGL-2.0 GLES-2.0, and choose all extension
4. Modify cocos2d-x sources to use glad, pitfall: ```glPolygonMode```, not present in GLES-2.0
5. Modify GLProgram.cpp set ```headersDef``` same with ANDROID_PLATFORM  
gclient error solution:  
For Chinese system language users:  
在 控制面板-区域-更改系统区域设置 中  
勾选 Beta版：使用 Unicode UTF=8 提供全球语言支持(U) 选项  
python ImportError: No module named win32file:  
python -m pip install pywin32  

## iOS xcodebuild export, xxx is not an 'iOS App Store' profile.
Add thinning=none to XXXExportOptions.plist

## build engine-x
### ios
```sh
mkdir build
cd build
cmake .. -G Xcode -DCMAKE_TOOLCHAIN_FILE=../cmake/ios.toolchain.cmake -DPLATFORM=OS -DENABLE_ARC=0 -DENABLE_BITCODE=0
```

## Build openal-soft for ios platform
### xcode11 + cmake-3.17.3 works well
### devenv setup
1. Install brew:
```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```  
2. Install tools:
```
brew update
brew install git
brew install cmake
brew install autoconf
brew install automake
brew install libtool
```
3. Clone ios-cmake from github: https://github.com/leetal/ios-cmake
4. The solution of CMake Error: iphoneos is not an iOS SDK:  
important: ```sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer``` . 
see also: https://github.com/leetal/ios-cmake/issues/52  
CMake Gen ios xcodeproject with default cmake toolchain:
```cmake .. -GXcode -DCMAKE_SYSTEM_NAME=iOS -DCMAKE_OSX_SYSROOT=iphoneos -DCMAKE_C_COMPILER=/Applications/Xcode.app/Contents/Developer/usr/bin/gcc -DCMAKE_CXX_COMPILER=/Applications/Xcode.app/Contents/Developer/usr/bin/g++```

5. openal: https://github.com/kcat/openal-soft
* Gen with custom ios-cmake toolchain
```sh
cmake -GXcode .. -DCMAKE_TOOLCHAIN_FILE=../cmake/ios.toolchain.cmake -DPLATFORM=OS -DCMAKE_CXX_EXTENSIONS=OFF -DALSOFT_REQUIRE_COREAUDIO=ON  -DALSOFT_EMBED_HRTF_DATA=YES -DLIBTYPE=STATIC -DALSOFT_UTILS=OFF -DALSOFT_EXAMPLES=OFF
cmake -GXcode .. -DCMAKE_TOOLCHAIN_FILE=../cmake/ios.toolchain.cmake -DPLATFORM=SIMULATOR64 -DCMAKE_CXX_EXTENSIONS=OFF -DALSOFT_REQUIRE_COREAUDIO=ON  -DALSOFT_EMBED_HRTF_DATA=YES -DLIBTYPE=STATIC -DALSOFT_UTILS=OFF -DALSOFT_EXAMPLES=OFF
```
* Gen combine .a to 1 flat
```sh
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/libtool -static /Users/${USER}/dev/openal-soft/build/OpenAL.build/Release-iphoneos/OpenAL.build/Objects-normal/armv7/libopenal.a /Users/${USER}/dev/openal-soft/build/OpenAL.build/Release-iphoneos/OpenAL.build/Objects-normal/arm64/libopenal.a /Users/${USER}/dev/openal-soft/build_x64/Release-iphonesimulator/libopenal.a -o /Users/${USER}/dev/openal-soft/build/Release-iphoneos/libopenal.a
```

* Gen with cmake official(**device doesn't work**)
```sh
cmake .. -GXcode -DCMAKE_SYSTEM_NAME=iOS -DCMAKE_OSX_SYSROOT=iphonesimulator13.6 -DLIBTYPE=STATIC -DALSOFT_UTILS=OFF -DALSOFT_EXAMPLES=OFF -DALSOFT_EMBED_HRTF_DATA=YES -DALSOFT_TESTS=OFF
```

### ogg
```sh
mkdir build
cmake .. -G Xcode -DCMAKE_TOOLCHAIN_FILE=../../ios-cmake/ios.toolchain.cmake -DPLATFORM=OS
cmake --build . --config Release --target ogg
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/libtool -static ./build/libogg.build/Release-iphoneos/ogg.build/Objects-normal/armv7/libogg.a ./build_x86/Release-iphonesimulator/libogg.a ./build_x64/Release-iphonesimulator/libogg.a ./build/libogg.build/Release-iphoneos/ogg.build/Objects-normal/arm64/libogg.a -o ./lib/libogg.a
```

### vorbis
```sh
mkdir build
cmake .. -G Xcode -DCMAKE_TOOLCHAIN_FILE=../../ios-cmake/ios.toolchain.cmake -DOGG_INCLUDE_DIRS=/Users/halx99/Documents/ogg/include -DOGG_LIBRARIES=/Users/halx99/Documents/ogg/lib/ -DPLATFORM=OS
cmake --build . --config Release --target vorbis vorbisfile
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/libtool -static ./build/lib/vorbis.build/Release-iphoneos/vorbis.build/Objects-normal/armv7/libvorbis.a ./build_x86/lib/Release-iphonesimulator/libvorbis.a ./build_x64/lib/Release-iphonesimulator/libvorbis.a ./build/lib/vorbis.build/Release-iphoneos/vorbis.build/Objects-normal/arm64/libvorbis.a -o ./libs/libvorbis.a
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/libtool -static ./build/lib/vorbis.build/Release-iphoneos/vorbisfile.build/Objects-normal/armv7/libvorbisfile.a ./build_x86/lib/Release-iphonesimulator/libvorbisfile.a ./build_x64/lib/Release-iphonesimulator/libvorbisfile.a ./build/lib/vorbis.build/Release-iphoneos/vorbisfile.build/Objects-normal/arm64/libvorbisfile.a -o ./libs/libvorbisfile.a
```

### check arch of .a
```lipo -info libogg.a```

## Use Visual Studio find chinese charactors regex expression
```sh
.*[\u4e00-\u9fa5]{1,}.*
```

## Android Studio maven private server
### alternative solution
* nexus: https://www.sonatype.com/nexus-repository-oss
* jfrog: https://jfrog.com/open-source
#### nexus
 ```nexus /install```  
 ```net start nexus```  
 package url format: http://127.0.0.1:9910/repository/maven-public/com/android/tools/lint/lint/26.2.1/lint-26.2.1.pom
### official
* google(): https://dl.google.com/dl/android/maven2/  
* jcenter(): https://jcenter.bintray.com/  
* mavenCentral(): https://repo.maven.apache.org/maven2/ or https://repo1.maven.org/maven2/ or http://central.maven.org/maven2/
### aliyun: 
* google(): http://maven.aliyun.com/nexus/content/repositories/google 
* jcenter(): http://maven.aliyun.com/nexus/content/repositories/jcenter

### disable domain at hosts: repo.maven.apache.org 151.101.52.215

### gradle:
https://services.gradle.org/distributions/


## wireshak cap
Use WinPcap 4.1.3

## global mobile games website:
https://apkpure.com/

## show vctools version option
-Bv

## git update submodule url
git submodule sync

## git remove submodule
git rm --cached {submodule_path}

## Chrome Latest Offline Download
[ChromeStandaloneSetup64.exe](https://www.google.cn/chrome/?standalone=1&platform=win64)

## Unity3D Paths
### Android
streamingAssetsPath: jar:file://${APK_PATH}!assets  
persistentDataPath: external storage path, Android/data/${PackageName}/files  
dataPath: ${APK_PATH}  

## Install latest cmake on non-win32 platform
```sh
apt install libssl-dev
wget https://github.com/Kitware/CMake/releases/download/v3.16.2/cmake-3.16.2.tar.gz -o ./cmake-3.16.2.tar.gz 
tar -xvf cmake-3.16.2.tar.gz
cd cmake-3.16.2
./configure
make
make install
```

## Upgrade ubuntu
```sh
apt update && apt dist-upgrade
apt-get install screen
screen # Avoid ssh connection lost
do-release-upgrade -d
```

## php xdebug
* Open phpForApache.ini
* Edit [xdebug] section
 ```ini
zend_extension="path/to/php/zend_ext/php_xdebug-2.8.0-7.3-vc15-x86_64.dll"
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
xdebug.romote_host=localhost
xdebug.remote_port=9000
 ```

