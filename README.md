[TOC]
### 1. 接入方式

将AntiCheating-release.aar、common-release.aar以及移动安全联盟的miit_mdid_1.0.10.aar拷贝至app module的libs下，在app的build.gradle添加依赖。
(注： 若已集成广告SDK，则不需要导入common库以及miit_mdid_1.0.10库)
```groovy
android {
    repositories {
        flatDir {
            dirs 'libs' // aar用到
        }
    }
		....
}

dependencies {
    implementation(name: 'common-release-[版本号]', ext: 'aar')
    implementation(name: 'miit_mdid_1.0.10', ext: 'aar')
    implementation(name: 'AntiCheating-release-[版本号]', ext: 'aar')
 		...
}
```

反作弊SDK中使用了移动安全联盟SDK中提供的OAID功能，因此在接入时要进行以下操作：

在app的main文件夹下创建assets目录，在该目录创建名为supplierconfig.json的文件，在文件中声明对应应用市场的appid，目前只有vivo的appid必须声明。

```json
{
  "supplier":{
    "vivo":{
      "appid":"100215079"
    },
    "xiaomi":{

    },
    "huawei":{

    },
    "oppo":{

    }
  }
}
```

### 2. 使用方式

在Application中进行初始化

```java
public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AntiCheatingSDK.init(this);
    }
}
```

在AndroidManifest.xml的application节点下使用meta-data配置渠道信息(value可以为空串，可以使用360加固包进行多渠道自动赋值)

```xml
<application
       ...
       android:name=".App">

  		<meta-data
            android:name="UMENG_CHANNEL"
            android:value="[渠道名]" />
</application>

              
```

### 3. 数据上报
#### 3.1 配置

在Application节点下配置appId

```xml
  <meta-data
            android:name="com.admobile.anticheat.APP_ID"
            android:value="[appId]" />
```

#### 3.2 数据上报API

数据以键值对的形式上报。

AntiCheatingSDK

```java
public static void track(String key, String value);

public static void track(String key, int value);
```





