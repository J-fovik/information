# 部署代码到线上服务器

01: 购买服务器

阿里云服务器官网:  https://www.aliyun.com/daily-act/ecs/activity_selection?utm_content=se_1012584386

 02: 购买完服务器, 然后点击远程连接, 连接并进入远程服务器.![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823527445468.png) 



03: 远程服务器终端

![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823528357463.png) 

04:  在远程服务器终端执行安装宝塔面板的命令

宝塔面板官网: https://www.bt.cn/new/index.html

宝塔面板 账户: 15510052553 密码: WuSheng19900201

 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823530654260.png) 



05: 根据对应的服务器操作系统选择对应的安装宝塔面板的脚本命令

 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823536047340.png) 

06: 安装完宝塔面板 使用命令行中的登录名和密码访问当前服务器中安装的宝塔面板

 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823560532533.png) 

07: 根据宝塔面板的提示推荐一键安装服务器中需要的软件, 就不需要使用linux 命令一个一个安装软件了

 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823562861160.png)  

08: 安装完对应的软件就可以 添加网站的站点了

点击 网站 => 添加站点
 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823570709509.png) 

 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823571758777.png) 



09: 在服务器的宝塔面板中上传 打包好的项目 npm run build 的目录
 ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823575561408.png) 

10. 在浏览器中浏览当前域名, 则可以看到对应的线上的项目了, 

    但是注意了: 有一个问题, 刷新出现了404 页面, 这是由于history 路由模式导致的

    解决方法: https://blog.csdn.net/weixin_45982458/article/details/127919310
     ![img](C:\Users\wusheng\AppData\Local\Temp\企业微信截图_16823579989262.png) 