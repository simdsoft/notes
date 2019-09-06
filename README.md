# notes

## wamp 3.x
### Access form other computer on the local area network
1. Change http.conf 'Require local' to 'Require all granted'
2. Check firewall strategy, ensure the httpd.exe in allow list.

## AudioEngine dependencies at android platform
### make-standalone-toolchain
"$ANDROID_NDK/build/tools/make-standalone-toolchain.sh" --arch="arm" --platform="14" --stl=gnustl --install-dir="/opt/arm-toolchain-r10e"

### openal-soft  
https://github.com/kcat/openal-soft  
cd build  
cmake .. -DCMAKE_TOOLCHAIN_FILE=../XCompile-Android.txt -DHOST=arm-linux-androideabi -DCMAKE_C_FLAGS="-mfloat-abi=softfp -mfpu=neon" -DCMAKE_BUILD_TYPE=Release

CMAKE_BUILD_TYPE options: Debug Release RelWithDebInfo MinSizeRel

### mpg123
download: http://www.mpg123.de/  
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc CXX=arm-linux-androideabi-g++ --with-cpu=neon

### libogg-1.3.3:
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc CXX=arm-linux-androideabi-g++

### libvorbis-1.3.6:
./configure --host=arm-linux-androideabi CC=arm-linux-androideabi-gcc CXX=arm-linux-androideabi-g++ --with-ogg-includes=/usr/local/src/libogg-1.3.3/include/ --with-ogg-libraries=/usr/local/src/libogg-1.3.3/src/.libs/

### strip symbols
remove static library debug symbol: arm-linux-androideabi-strip -g -S -d --strip-debug libogg.a -o libogg-r.a  
remove dynamic library debug symbol: arm-linux-androideabi-strip libopenal.so -o /mnt/hgfs/ushared/libopenal.so


## readthedocs dev
1. Install [Python-3.7.3](https://www.python.org/ftp/python/3.7.3/python-3.7.3-amd64.exe)
2. Install Python Extensions: sphinx, sphinx-autobuild, doc8: ```pip install sphinx sphinx-autobuild doc8 sphinx_tabs``` for reStructedText preview.
For more detail, see: https://docs.restructuredtext.net/articles/prerequisites.html
3. Install [Microsoft VS Code](https://code.visualstudio.com/)
4. Install Microsoft VS Code extension: reStructedText, just search ```rst```

## Use google angle GLES-2.0 in cocos2d-x
1. Build angle latest source from google
2. Copy libGLESv2.dll, libEGL.dll to your game directory
3. Use glad to generate with OpenGL-2.0 GLES-2.0, and choose all extension
4. Modify cocos2d-x sources to use glad, pitfall: ```glPolygonMode```, not present in GLES-2.0
5. You need use [gles2gl](https://github.com/x-studio365/gles2gl) to translate GLES API to GL API at runtime.
6. Modify GLProgram.cpp set ```headersDef``` same with ANDROID_PLATFORM

## iOS xcodebuild export, xxx is not an 'iOS App Store' profile.
Add thinning=none to XXXExportOptions.plist
