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
2. Install Python Extensions: sphinx, sphinx-autobuild, doc8: ```pip install sphinx sphinx-autobuild doc8``` for reStructedText preview.
3. Install [Microsoft VS Code](https://code.visualstudio.com/)
4. Install Microsoft VS Code extension: reStructedText, just search ```rst```
