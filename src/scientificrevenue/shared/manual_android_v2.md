### 拷贝文件
从插件安装包中的 `plugin/android/libs` 目录拷贝下列 jar 文件到您的工程的 __proj.android/libs__ 目录。

> ScientificRevenueSDK-Android-1.0.0.jar

> PluginScientificRevenue.jar

> sdkbox.jar

从 `plugin/android/jni` 目录下拷贝 `PluginScientificRevenue` 和 `sdkbox` 目录到您工程的 `proj.android/jni/` 目录。如果 `sdkbox` 目录已经存在，那么覆盖它。

### 编辑 `AndroidManifest.xml`
在标签 __application tag__ 上添加下列权限：
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="com.android.vending.BILLING"/>
<uses-permission android:name="android.permission.GET_TASKS"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

在标签 __application tag__ 下添加以下 service ：
```xml
<service android:exported="false" android:label="PricingService" android:name="com.scientificrevenue.service.PricingService"/>
```


### 编辑 `Android.mk`
编辑 `proj.android/jni/Android.mk`：

为 __LOCAL_WHOLE_STATIC_LIBRARIES__ 添加额外的库：
```
LOCAL_WHOLE_STATIC_LIBRARIES += android_native_app_glue
LOCAL_LDLIBS += -landroid
LOCAL_LDLIBS += -llog
LOCAL_WHOLE_STATIC_LIBRARIES += PluginScientificRevenue
LOCAL_WHOLE_STATIC_LIBRARIES += sdkbox
```

在所有 __import-module__ 语句之前添加一条 call 语句：
```
$(call import-add-path,$(LOCAL_PATH))
```

在最后添加额外的 __import-module__ 语句：
```
$(call import-module, ./sdkbox)
$(call import-module, ./PluginScientificRevenue)
```

这意味着您的语句顺序看起来像是这样：
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./PluginScientificRevenue)
```

### 编辑 `Application.mk`
编辑 `proj.android/jni/Application.mk`：

为 __APP_PATFORM__ 添加版本：
```
APP_PLATFORM := android-9
```
