# notes

## wampserver 3.x
### Access form other computer on the local area network
1. Change http.conf ```Require local``` to ```Require all granted```
1. Change extra/http-vhosts.conf ```Require local``` to ```Require all granted```
2. Check firewall strategy, ensure the httpd.exe in allow list.

## Build openal-soft dependencies for android platform
### OS: Ubuntu 18.04.3 or install it with VMWare

### install dep-tools
```apt-get install autoconf automake cmake libtool git```

### make-standalone-toolchain
prepare: ```cp /usr/bin/python3.6 /usr/bin/python```  
```"$ANDROID_NDK/build/tools/make-standalone-toolchain.sh" --arch="arm" --platform="14" --stl=gnustl --install-dir="/opt/arm-standalone-toolchain" --verbose```  
```"$ANDROID_NDK/build/tools/make-standalone-toolchain.sh" --arch="arm64" --platform="21" --stl=gnustl --install-dir="/opt/arm64-standalone-toolchain"  --verbose```

### Add standalone toolchain's bin directory to PATH
```export PATH=/opt/arm-standalone-toolchain/bin:$PATH```
```export PATH=/opt/arm64-standalone-toolchain/bin:$PATH```

### openal-soft  
```git clone https://github.com/halx99/openal-soft```  
```git checkout v1.19-aspec```  
```export CMAKE_FIND_ROOT_PATH=/opt/arm-standalone-toolchain```  
```export CMAKE_FIND_ROOT_PATH=/opt/arm64-standalone-toolchain```  
cd build  
```cmake .. -DCMAKE_TOOLCHAIN_FILE=../XCompile-Android.txt -DHOST=arm-linux-androideabi -DCMAKE_C_FLAGS="-mfloat-abi=softfp -mfpu=neon" -DCMAKE_BUILD_TYPE=Release```  
```cmake .. -DCMAKE_TOOLCHAIN_FILE=../XCompile-Android.txt -DHOST=aarch64-linux-android -DCMAKE_BUILD_TYPE=Release```  

CMAKE_BUILD_TYPE options: Debug Release RelWithDebInfo MinSizeRel

### mpg123
download: http://www.mpg123.de/  
```./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc --with-cpu=neon```  
```./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc --with-cpu=neon64```

### libogg-1.3.4:
```./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc```  
```./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc```

### libvorbis-1.3.6:
```./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.4/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.4/src/.libs/```  
```./configure --host=aarch64-linux-android CC=aarch64-linux-android-gcc --with-ogg-includes=/usr/local/src/libogg-1.3.4/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.4/src/.libs/```

### strip symbols
remove static library debug symbol: ```arm-linux-androideabi-strip -g -S -d --strip-debug libogg.a -o libogg-r.a```  
remove dynamic library debug symbol: ```arm-linux-androideabi-strip libopenal.so -o /mnt/hgfs/ushared/libopenal.so```


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

## iOS xcodebuild export, xxx is not an 'iOS App Store' profile.
Add thinning=none to XXXExportOptions.plist

## Use Visual Studio find chinese charactors regex expression
```.*[\u4e00-\u9fa5]{1,}.*```

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
