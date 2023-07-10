# Hybrid-offline-ocat
修改的一种混合开发，离线包方案，原作者Ocat
主要解决：加载web时的白屏、加载慢造成的用户体验不佳
主要原理：静态资源在本地加载（解决网络状态、加载缓慢等影响），为了解决离线包的管理，进行版本间差分。

## 步骤：
# 1、部署Docker
cd 到 docker文件夹
执行 docker-compose up -d
结果：看到docek中容器启动，nginx，和mysql

# 2、初始化数据库有
使用数据库管理工具打开docker中的数据库
用户名：root
密码：root
连接上后，打开database，依次执行ocat.sql、rbac.sql、init.sql
结果：检查数据库中，创建了对应的表和初始数据

# 3、打开mobileplatform工程（java工程）
修改mobileplatform/src/main/resources/application-dev.yml
修改如下：
        workshop-path: /Users/black2w/Downloads/OCat-MobilePlatform-master/mobileplatform/workshop # 差量比较工作目录
        online-url: http://172.17.16.191:8080
        package-download-path: /Users/black2w/Downloads/OCat-MobilePlatform-master/docker/nginx/html/download/packages # 可以放到云上
        online-version-path: /Users/black2w/Downloads/OCat-MobilePlatform-master/docker/nginx/html/online
        prep-online-version-path: /Users/black2w/Downloads/OCat-MobilePlatform-master/docker/nginx/html/preview

工程中接口支持跨域        
结果：启动运行，浏览器中输入127.0.0.1:9090（或localhost），提示需要登录

# 4、打开mobileplatform-portal工程（vue工程）
npm run serve

结果：启动运行，浏览器中输入127.0.0.1:8082(或localhost),打开登录页。
输入用户名：S-administrator
密码：a123  
或者
输入用户名：package-administrator
密码：a321
可以登录

# 5、在步骤4的基础上创建APP，获得appkey、appsecert
上传版本1.0.0（文件地址：OCat-MobilePlatform-master/test-package/1.0.0.zip）

# 6、打开对应的android-demo、IOS-demmo
程序中appkey和appsecert填入步骤5生成的相关数据。
对应代码：
IOS：
serverBaseUrl：接口地址，使用步骤3服务的ip地址（ifconfig获得）
kOnlineServerUrl：线上地址，使用步骤4服务的ip地址（ifconfig获得）


[OCatConfiguration initWithAppId:@"3154908711"
                                                              appSecret:@"564a93a0c1604ff383243f0dd55df326"
                                                          serverBaseUrl:@"http://172.17.16.191:9090"
                                                  inbuiltPackageVersion:@"1.0.0"];
                                                  
static NSString *const kOnlineServerUrl = @"http://172.17.16.191:8080";

Android:参照ios
/Users/black2w/Downloads/OCat-MobilePlatform-master/android-demo/build.gradle中修改成环境对应的版本：
 dependencies {
        classpath 'com.android.tools.build:gradle:7.0.0'
        

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
    
        private static final String ONLINE_URL = "http://172.17.16.191:8080";
        
                PackageSettings packageSettings = new PackageSettings(
                "3154908711",
                "564a93a0c1604ff383243f0dd55df326",
                "http://172.17.16.191:9090",
                PRE_PACKAGE_VERSION);
                                        
                                        
# 7、运行相应的客户端程序

### 注意事项：
1、上传的压缩文件需要在windows下面压缩，mac下面压缩上传会后台崩溃
2、注意接口的跨域调用、vue调试时需要打开代理
3、客户端本地加载时，js调用接口的数据是否可正常加载？在研究中
4、客户端本地加载时，js加载数据是否可以优化，节约数据加载时间。研究中（加载与数据请求并发进行，一种思路）
5、开发实力足够的出路应该在类似于小程序的架构设计上。

black2w@126.com(17714007899)，欢迎大家一起讨论