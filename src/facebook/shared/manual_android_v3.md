## Android 平台手动集成

### 拷贝文件
从插件安装包中的 `plugin/android/libs` 目录拷贝下列 __jar__ 文件到您的工程的 __proj.android/libs__ 目录。

> PluginFacebook.jar

> sdkbox.jar

<<[../../shared/copy_jars.md]

从 `plugin/android/jni` 目录拷贝 `pluginfacebook` 以及 `sdkbox` 目录到您的工程的 `proj.android/jni` 目录。如果 `sdkbox` 目录在工程中已经存在，请覆盖它。

从 `plugin/android/libs` 拷贝 `facebook_lib` 目录到您的 `proj.android/libs/` 目录。


### 编辑 `AndroidManifest.xml`
在标签 __application tag__ 上添加下列权限：
```xml
  <uses-permission android:name="android.permission.INTERNET" />
```

还有一些必要的元数据标签需要添加：
```xml
<meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id"/>
<activity android:name="com.facebook.FacebookActivity"
  android:configChanges=
         "keyboard|keyboardHidden|screenLayout|screenSize|orientation"
  android:theme="@android:style/Theme.Translucent.NoTitleBar"
  android:label="@string/app_name" />

  <provider android:authorities="com.facebook.app.FacebookContentProvider__replace_with_your_app_id__"
  android:name="com.facebook.FacebookContentProvider"
  android:exported="true" />
```

### 编辑 strings.xml
打开 `res/values/strings.xml`，添加一个新的名为 `facebook_app_id` 的字串，其值是您的 Facebook App ID。比如：

  ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <string name="app_name">facebook</string>
        <string name="facebook_app_id">280194012150923</string>
    </resources>
  ```

### 编辑 `Android.mk`
编辑 `proj.android/jni/Android.mk`：

为 __LOCAL_STATIC_LIBRARIES__ 添加额外的库：
```
LOCAL_STATIC_LIBRARIES += PluginFacebook
LOCAL_STATIC_LIBRARIES += sdkbox
```

在所有 __import-module__ 语句之前添加一条 call 语句：
```
$(call import-add-path,$(LOCAL_PATH))
```

在最后添加额外的 __import-module__ 语句：
```
$(call import-module, ./sdkbox)
$(call import-module, ./pluginfacebook)
```

这意味着您的语句顺序看起来像是这样：
```
$(call import-add-path,$(LOCAL_PATH))
$(call import-module, ./sdkbox)
$(call import-module, ./pluginfacebook)
```

  __Note:__ 如果您使用的是 cocos2d-x 预编译库，那么保证这些语句在已有的 `$(call import-module,./prebuilt-mk)` 语句之上非常重要。

### 编辑 `Application.mk` （只限 Cocos2d-x v3.0 到 v3.2 版本）
编辑 `proj.android/jni/Application.mk` 保证 __APP_STL__ 的定义正确。如果 `Application.mk` 包含了 `APP_STL := c++_static` 语句，那么这条语句应该被改为：
```
APP_STL := gnustl_static
```
