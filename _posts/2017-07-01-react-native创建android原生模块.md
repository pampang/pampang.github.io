# # 参考文档
下文讲的非常详细，很多方法都说明了，仔细再看：
http://www.liuchungui.com/blog/2016/05/08/reactnativezhi-yuan-sheng-mo-kuai-kai-fa-bing-fa-bu-androidpian/

ios原生模块创建：
http://www.liuchungui.com/blog/2016/05/02/reactnativezhi-yuan-sheng-mo-kuai-kai-fa-bing-fa-bu-iospian/

极光推送的实例：
https://community.jiguang.cn/t/react-native-android/12126

# 具体操作

## 1、在mall下创建一个module

#### 1) 需要切换到project目录结构，然后对着mall来进行创建。或者直接在android目录结构下，空白位置右键，new一个module出来。

![android1](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android1.png)

![android2](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android2.png)


找到module的位置，然后将其迁移到对应的文件夹中，并将文件夹改名为android

#### 2) 在app/build.gradle下面，添加compile project

![android3](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android3.png)


#### 3) 打开新创建的module下的build.gradle，加入必须的compile project

![android4](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android4.png)

其中，react-native是必须的，一定要有的。
然后，可以添加Module下所需的compile依赖，并添加依赖额外安装的路径（mavenCentral()）


#### 4) 在新创建的module下创建对应的java文件，包括：

```
1/ module文件： RNUmengAnalytics[Module]
2/ package文件：RNUmengAnalyticsPackage
```

![android5](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android5.png)

其中，module文件里面初始化的代码包括：

![android6](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android6.png)

```
package com.pampang.RNUmengAnalytics;

import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;

/**
* Created by PAMPANG on 2016/11/24.
*/

public class RNUmengAnalytics extends ReactContextBaseJavaModule {

    // 构造方法
    public RNUmengAnalytics(ReactApplicationContext reactContext) {
        super(reactContext);
    }

    // 覆写getName方法，它返回一个字符串名字，在JS中我们就使用这个名字调用这个模块
    @Override
    public String getName() {
        return "UmengAnalytics";
    }
}
```

package文件的初始化代码包括：

![android7](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android7.png)

```
package com.pampang.RNUmengAnalytics;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.JavaScriptModule;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

/**
* Created by PAMPANG on 2016/11/24.
*/

public class RNUmengAnalyticsPackage implements ReactPackage {

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        return Arrays.asList(new NativeModule[] {
                new RNUmengAnalytics(reactContext)
        });
    }

    @Override
    public List<Class<? extends JavaScriptModule>> createJSModules() {
        return Collections.emptyList();
    }

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }
}
```

#### 5) 在MainApplication里添加新创建的package

![android8](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android8.png)

#### 6) 迁移时刻
将创建的module迁移到node_modules下，再从node_modules引入该项目。
新创建的module会出现在android/目录下。
直接将其剪切到对应的npm仓库里面去，将目录名字改为android。
在android/settings.gradle里面，引入对应的Module:

![android9](http://osd9kk2in.bkt.clouddn.com/2017-07-01-android9.png)

# 再次gradle sync，大功告成！


