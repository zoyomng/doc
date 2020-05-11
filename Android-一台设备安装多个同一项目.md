## Android-在一台Android设备安装多个同一项目的apk

**在app下面的bulid.gradle中添加如下代码:**

1. 第一种方法

```
 buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug {
            applicationIdSuffix ".free"
        }
    }
}
```

- `applicationIdSuffix ".free"`这一行关键的代码
- 包名变成了`packageName.free`

 

2. 第二种方法

```
     productFlavors {
        pro {
            applicationId = "com.myApp.pro"
        }
        free {
            applicationId = "com.myApp.free"
        }
    }
```



3. 如果两种方法同时使用

```
     productFlavors {
        pro {
            applicationId = "com.myApp.pro"
        }
        free {
            applicationId = "com.myApp.free"
        }
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug {
            applicationIdSuffix ".test"
        }
    }
```

- 如果同时使用两种方式，buildTypes 中的设置后缀会跟在productFlavors 中设置的包名后面,这时候包名就会变成`com.myApp.pro.test`和`com.myApp.free.test`.

- `Application ID`可以说是一个android项目的唯一标识，是不可修改的, 并且Application ID和package的属性值不一样的，虽然不一样，但是当我们在构建项目的时候，会复制Application ID给package的属性值,并且这个属性值是唯一的，package属性才是真正作为您应用程序唯一身份凭证。



