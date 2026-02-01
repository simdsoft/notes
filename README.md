# notes

## emsdk

- emsdk-5.0.0
    - linux: https://storage.googleapis.com/webassembly/emscripten-releases-builds/linux/e44d3cc557d78155966478aa2bd8dec657609619/wasm-binaries.tar.xz

## uninsatll old kernrl

卸载 Ubuntu 的旧版本内核可以释放磁盘空间并优化系统性能。以下是几种常见方法：

1. **自动卸载未使用的内核**：
   - 打开终端，运行以下命令查看已安装的内核列表：
     ```bash
     dpkg --list | grep linux-image
     ```
   - 然后运行以下命令自动卸载未使用的内核：
     ```bash
     sudo apt autoremove --purge
     ```

2. **手动删除特定内核**：
   - 首先运行以下命令查看当前正在使用的内核版本：
     ```bash
     uname -r
     ```
   - 确保不要删除当前使用的内核。接着运行以下命令列出所有已安装的内核：
     ```bash
     dpkg --list | grep linux-image
     ```
   - 使用以下命令删除指定版本的内核（替换 `x.x.x-x` 为实际版本号）：
     ```bash
     sudo apt purge linux-image-x.x.x-x-generic
     ```
   - 删除后，更新 GRUB 引导加载程序：
     ```bash
     sudo update-grub
     ```
   - 最后，重新启动系统。

3. **清理残留的配置文件**：
   - 使用以下命令清除所有状态为 `rc` 的包：
     ```bash
     sudo dpkg --purge $(dpkg -l | awk '/^rc/{print $2}')
     ```

在操作之前，请备份重要数据，并保留至少一个旧版本内核以防万一。如果需要更详细的指导，可以参考[这里](https://bing.com/search?q=ubuntu+%e5%a6%82%e4%bd%95%e5%8d%b8%e8%bd%bd%e6%97%a7%e7%89%88%e6%9c%ac%e5%86%85%e6%a0%b8)或[这里](http://www.linuxrtm.com/c/22291.html)。

希望这些方法对你有所帮助！如果有其他问题，随时告诉我。 😊

## adb list package

```pwsh
adb shell pm list packages | findstr "xxx"
adb shell pm path com.xxx
adb pull xxxx ./
```

## adb connect to MuMu emulator

`adb connect 127.0.0.1:7555`

## Force switch Windows 11 insider to Release Preview

1. Ensure go back to release preview build

Methd1:  
按Win+R输入regedit打开注册表  

定位路径：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsSelfHost\UI\Selection  

修改UIBranch值为ReleasePreview  

修改ContentType值为Mainline  

修改Ring和UIRing值为Retail  

定位路径：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsSelfHost\Applicability  

修改BranchName值为ReleasePreview  

修改ContentType值为Mainline  

修改Ring值为Retail  

重启系统后检查设置-更新-Insider Program是否变为Release Preview  

Method2. https://github.com/abbodi1406/offlineinsiderenroll  


## install vs2022 specific version

1. Locate the particular version in the Microsoft Update Catalog and download the installer.
2. Extract ChannelManifest.json from the installer. Should be about 90 KiB.
3. Upload it to a web server so that you have a URI it could be downloaded from, say https://example.org/ChannelManifest.json.
4. Go to C:\Program Files (x86)\Microsoft Visual Studio\Installer and open the command line there.
5. Run setup update --channelId VisualStudio.17.Release --productId Microsoft.VisualStudio.Product.Community --channelURI "https://example.org/ChannelManifest.json". Feel free to add other parameters.

## install xcode manually

### requires

```sh
brew install unxip
```

1. visit: https://developer.apple.com/download/all/
2. search `xcode 14.2`
3. click .xip to download

xcode-14.2

```sh
# extract it
cd ~/Downloads/
xip -x ~/Downloads/Xcode_14.2.xip
mv Xcode.app /Applications/Xcode_14.2.app
sudo xcode-select -switch /Applications/Xcode_14.2.app
```

xcode-26.x

```sh
cd ~/Downloads/

# overwrite, please uninstall /Applications/Xcode.app manually first
unxip ~/Downloads/Xcode_26.2_Universal.xip /Applications/

# multi install
mkdir xcode-26.2-extract
unxip ~/Downloads/Xcode_26.2_Universal.xip /xcode-26.2-extract/
mv /xcode-26.2-extract/Xcode.app /Applications/Xcode_26.2.app
```

## 通过wsl2访问Linux物理磁盘
https://devblogs.microsoft.com/commandline/access-linux-filesystems-in-windows-and-wsl-2/

## ubuntu nvdia driver

命令行安装ok
https://blog.csdn.net/qq_34972053/article/details/127689332

chrome安装

vscode安装

缺x11

## centos6.10 install gcc5
1. Add repo (important)
```sh
vim /etc/yum.repos.d/devtools-4.repo
# input follow content
[hhorak-devtoolset-4-rebuild-bootstrap]
name=Copr repo for devtoolset-4-rebuild-bootstrap owned by hhorak
baseurl=https://copr-be.cloud.fedoraproject.org/results/hhorak/devtoolset-4-rebuild-bootstrap/epel-6-$basearch/
type=rpm-md
skip_if_unavailable=True
gpgcheck=1
gpgkey=https://copr-be.cloud.fedoraproject.org/results/hhorak/devtoolset-4-rebuild-bootstrap/pubkey.gpg
repo_gpgcheck=0
enabled=1
enabled_metadata=1
```
2. install
```
# install scl
yum -y install centos-release-scl
# install gcc-5 from devtoolset-4
yum -y install devtoolset-4-gcc devtoolset-4-gcc-c++ devtoolset-4-binutils
# enable temporary
scl enable devtoolset-4 bash
gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/opt/rh/devtoolset-4/root/usr/libexec/gcc/x86_64-redhat-linux/5.2.1/lto-wrapper
Target: x86_64-redhat-linux
Configured with: ../configure --enable-bootstrap --enable-languages=c,c++,fortran,lto --prefix=/opt/rh/devtoolset-4/root/usr --mandir=/opt/rh/devtoolset-4/root/usr/share/man --infodir=/opt/rh/devtoolset-4/root/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-shared --enable-threads=posix --enable-checking=release --enable-multilib --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --enable-plugin --with-linker-hash-style=gnu --enable-initfini-array --disable-libgcj --with-default-libstdcxx-abi=gcc4-compatible --with-isl=/builddir/build/BUILD/gcc-5.2.1-20150902/obj-x86_64-redhat-linux/isl-install --enable-libmpx --with-mpc=/builddir/build/BUILD/gcc-5.2.1-20150902/obj-x86_64-redhat-linux/mpc-install --with-tune=generic --with-arch_32=i686 --build=x86_64-redhat-linux
Thread model: posix
gcc version 5.2.1 20150902 (Red Hat 5.2.1-2) (GCC)
```
2. set gcc-5 as default
```sh
echo "source /opt/rh/devtoolset-4/enable" >>/etc/profile
```

## mac java
```sh
vim ~/.zshrc
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
export CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
source ~/.zshrc
```

## git push
```sh
git push origin --tags -f
```

## gsxt
http://www.gsxt.gov.cn/

## jar recompress
```sh
cd xxx
jar -xf xxx.jar
mv xxx.jar ../
jar -cvfm xxx.jar META-INF/MANIFEST.MF  ./
```

## clang arm predefininations
- clang -arch arm64 -E -dM - < /dev/null | grep arm
```cpp
#define __arm64 1
#define __arm64__ 1
```

## python mirrors
* http://npm.taobao.org/mirrors/python

## centos7 install gitea
```
adduser -r -s /bin/bash -c 'Git Version Control' -U -m git
```

## ios debug devices
https://www.jianshu.com/p/8c99cb119bb3

## Upgrade brew with mirrors
Install: https://gitee.com/simdsoft/brew-install  
Update: modify /usr/local/Homebrew/Library/Homebrew/install.sh  
change homebrew-core repo to https://gitee.com/simdsoft/homebrew-core  
https://www.jianshu.com/p/8a2ac505ff3e  
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

## Create Visual Studio 2022 offline installer
For C++ desktop development, run:
```bat
vs_community.exe --layout D:\softwares\2022 --add Microsoft.VisualStudio.Workload.NativeDesktop --includeRecommended --lang en-US
```
For .Net & C++ desktop development, run:
```bat
vs_community.exe --layout D:\softwares\2022 --add Microsoft.VisualStudio.Workload.NativeDesktop --add Microsoft.VisualStudio.Workload.ManagedDesktop --includeRecommended  --lang en-US
```

## build apk, config noCompress file type at app/build.gradle
```gradle
android {
    aaptOptions {
       noCompress 'mp3','ogg','wav','mp4','ttf','ttc'
    }
}
```

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

## iOS xcodebuild export, xxx is not an 'iOS App Store' profile.
Add thinning=none to XXXExportOptions.plist

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

