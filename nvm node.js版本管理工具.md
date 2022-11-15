## nvm 使用

**nvm 是什么？** 

nvm，node.js 版本管理工具



**nvm 有什么用？**

通过 nvm 可以安装和切换不同版本的node.js，解决 node.js 各种版本存在不兼容的问题



**安装 nvm**

nvm 的具体的安装流程可以看 [nvm的官方介绍](http://nvm.uihtm.com/)

> 注意：

1. 打开CMD，输入命令 nvm ，安装成功则会显示当前的 nvm 版本
2. 请先清理掉本地的 node，再在利用 nvm 下载需要使用的 node 版本



**nvm 常用的一些命令**

- nvm -v ： 查看 nvm 的版本

- nvm list available ： 查看可下载版本的部分列表
- nvm install 版本号 ： 安装指定的版本的 nodejs
- nvm ls ： 查看已经安装的版本
  - 带 * 的表示当前在使用的版本，
  - 如果没有一个版本号前面有 * 星号，表示还没有使用任何一个 node 版本
- nvm use 版本号 ：使用指定的 node 版本
- nvm uninstall 版本号 ： 卸载指定的 node 版本
- nvm on ： 启动 nvm 
- nvm off ： 关闭 nvm

