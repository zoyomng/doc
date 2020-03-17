1.首先从github上克隆flutter,这样的好处:便于flutter版本的更新
git clone -b stable https://github.com/flutter/flutter.git
2.配置环境变量
~$ sudo gedit /etc/profile

配置参数:
#jdk环境变量
export JAVA_HOME=/opt/java/jdk1.8.0_231
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#AndroidStudio环境变量
export ANDROID_SDK_ROOT=/opt/Sdk
export PATH=$PATH:${ANDROID_SDK_ROOT}/tools
export PATH=$PATH:${ANDROID_SDK_ROOT}/platform-tools

#Flutter环境变量
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PATH=/opt/flutter/bin:$PATH

以上根据自己jdk/AndroidStudio-Sdk/Flutter-Sdk所在目录配置

其中
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
为国内镜像配置

~$ source /etc/profile
保存刷新

3.~$ flutter doctor
会对flutter进行诊断
3.1如果缺少资源会自动下载:
Flutter assets will be downloaded from https://storage.flutter-io.cn. Make sure
you trust this source!
3.2其他诊断
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.12.13+hotfix.8, on Linux, locale zh_CN.UTF-8)
 
[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.3)
[!] Android Studio (version 3.6)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Community Edition (version 2019.2)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] Connected device
    ! No devices available
AndroidStudio未安装flutter插件等等

4.~$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.12.13+hotfix.8, on Linux, locale zh_CN.UTF-8)
 
[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.3)
[✓] Android Studio (version 3.6)

表示成功




