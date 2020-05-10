### 配置环境变量
`~$ sudo gedit /etc/profile`

配置参数:

- jdk环境变量

  ```
  export JAVA_HOME=/opt/java/jdk1.8.0_231
  export JRE_HOME=${JAVA_HOME}/jre
  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
  export PATH=${JAVA_HOME}/bin:$PATH
  ```

- AndroidStudio环境变量

  ```
  export ANDROID_SDK_ROOT=/opt/Sdk
  export PATH=$PATH:${ANDROID_SDK_ROOT}/tools
  export PATH=$PATH:${ANDROID_SDK_ROOT}/platform-tools
  ```

- Flutter环境变量

  ```
  export PUB_HOSTED_URL=https://pub.flutter-io.cn
  export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
  export PATH=/opt/flutter/bin:$PATH
  ```

  

*以上根据自己jdk/AndroidStudio-Sdk/Flutter-Sdk所在目录配置*


