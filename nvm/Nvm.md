# Nvm 的安装及使用 (Nodejs 版本管理器)

## 1、NVM 介绍

  在我们的日常开发中经常会遇到这种情况：手上有好几个项目，每个项目的需求不同，进而不同项目必须**依赖不同版的 NodeJS** 运行环境。如果没有一个合适的工具，这个问题将非常棘手。对于维护多个版本的[node](https://xie.infoq.cn/link?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3Dnode%26spm%3D1001.2101.3001.7020)将会是一件非常麻烦的事情，nvm 就是为解决这个问题而产生的，他可以方便的在同一台设备上进行多个 node 版本之间切换。

## 2、下载安装

下载地址：[https://github.com/coreybutler/nvm-windows/releases](https://xie.infoq.cn/link?target=https%3A%2F%2Fgithub.com%2Fcoreybutler%2Fnvm-windows%2Freleases)

![img](https://static001.geekbang.org/infoq/87/876c9a8686329a51bced52bef7a84271.png)



**注意事项：**

推荐卸载以及安装的 nodejs，毕竟以后要下载多版本，不差这一个。

安装目录尽量**英文**，安装时用**管理员**运行安装文件。



**安装**

1、解压 nvm-setup.zip 安装包，进入解压的文件夹，双击 exe 后缀文件进行安装，（**管理员运行**）

![img](https://static001.geekbang.org/infoq/05/0573c00e67642d1afa66223d09967cb5.png)



2、选择安装 nvm 的路径，自己可以更改安装路径，也可以默认路径。

![img](https://static001.geekbang.org/infoq/ac/ac883a7aa2c8154387e69b99679fb411.png)



3、这个是 nodejs 的安装位置尽量同级目录他俩

![img](https://static001.geekbang.org/infoq/e1/e1d381be2965555d56ae358532aa7713.png)

![img](https://static001.geekbang.org/infoq/ee/ee6b61ae7c36cadf3569e42ac8ed7150.png)



4、安装完毕后输入 nvm -v 查看版本。

![img](https://static001.geekbang.org/infoq/fc/fc70ed3717e7136dd03c342ec4f0e27d.png)



5、下面设置 setting.txt

```sh
root: D:\Software\nvm\nvm     --->路径
path: D:\Software\nodejs      --->nodejs路径

node_mirror: http://npm.taobao.org/mirrors/node/  #下面两行自己复制进去，用于加速
npm_mirror: https://npm.taobao.org/mirrors/npm/

```







## 3、NVM 使用

**最常用**其实只有：

------

- nvm list     有哪些
- nvm install < version >   下载
- nvm uninstall < version >   卸载
- nvm use < version >   用哪个

------



```js
nvm list //查看已安装的nodejs版本
nvm on // 启用node.js版本管理
nvm off // 禁用node.js版本管理(不卸载任何东西)
nvm install <version> // 安装node.js的命名 version是版本号 例如：nvm install 8.12.0 16.14.2
nvm use <version> //使用某一version的nodejs
nvm uninstall <version> // 卸载指定版本的nodejs
npm i <package> //安装包可以指定后缀，如-g、--save 、-dev等
npm r <package> //移除安装包
npm list -g //查看全局安装包

npm config set registry http://registry.npm.taobao.org/        #npm设置镜像
npm config get registry
nvm list //查看已安装的nodejs版本nvm on // 启用node.js版本管理nvm off // 禁用node.js版本管理(不卸载任何东西)nvm install <version> // 安装node.js的命名 version是版本号 例如：nvm install 8.12.0 16.14.2nvm use <version> //使用某一version的nodejsnvm uninstall <version> // 卸载指定版本的nodejsnpm i <package> //安装包可以指定后缀，如-g、--save 、-dev等npm r <package> //移除安装包npm list -g //查看全局安装包npm config set registry http://registry.npm.taobao.org/        #npm设置镜像npm config get registry
```



git-hub 官方文档



- **nvm arch [32|64]**：显示节点是在 32 位还是 64 位模式下运行。指定 32 或 64 以覆盖默认架构。
- **nvm current**：显示活动版本。
- **nvm install <version> [arch]**：版本可以是特定版本，“latest”表示最新的当前版本，或“lts”表示最新的 LTS 版本。可选地指定是安装 32 位还是 64 位版本（默认为系统架构）。将 [arch] 设置为“all”以安装 32 位和 64 位版本。添加`--insecure`到此命令的末尾以绕过远程下载服务器的 SSL 验证。
- **nvm list [available]**：列出 node.js 安装。在末尾键入`available`以显示可供下载的版本列表。
- **nvm on**: 启用 node.js 版本管理。
- **nvm off**：禁用 node.js 版本管理（不卸载任何东西）。
- **nvm proxy [url]**：设置用于下载的代理。留空`[url]`以查看当前代理。设置`[url]`为“无”以删除代理。
- **nvm uninstall <version>**：卸载特定版本。
- **nvm use <version> [arch]**：切换到使用指定的版本。可选择使用`latest`,`lts`或`newest`. `newest`是最新*安装*的版本。可选择指定 32/64 位架构。`nvm use <arch>`将继续使用所选版本，但切换到 32/64 位模式。有关`use`在特定目录中使用（或使用`.nvmrc`）的信息，请参阅[问题 #16](https://xie.infoq.cn/link?target=https%3A%2F%2Fgithub.com%2Fcoreybutler%2Fnvm-windows%2Fissues%2F16)。
- **nvm root <path>**: 设置 nvm 应该存放不同版本的 node.js 的目录。如果`<path>`未设置，将显示当前根目录。
- **nvm version**：显示当前运行的 NVM for Windows 版本。
- **nvm node_mirror <node_mirror_url>**：设置节点镜像。国内的人可以使用 *https://npmmirror.com/mirrors/node/*
- **nvm npm_mirror <npm_mirror_url>**：设置 npm 镜像。中国的人可以使用 *https://npmmirror.com/mirrors/npm/*

------

# 总结

>   NVM 全称 node.js version management ，专门针对 node 版本进行管理的工具，通过它可以安装和切换不同版本的 node.js。