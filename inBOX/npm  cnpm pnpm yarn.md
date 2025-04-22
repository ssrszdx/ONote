# npm  cnpm pnpm yarn

npm
npm是node.js自带的包管理工具，围绕语义版本控制
npm中有三种版本号：

主版本号：5.1.0当主API改变，并与之前的版本号不兼容的时候
次版本号：当增加了功能，但是向后兼容的时候
补丁版本号：当做了向后兼容但是版本有缺陷的时候
这就导致不同的人下载的版本号不同，可能会出现问题

npm2的时候，经常出现某个依赖包还需要另一个依赖包来支持，这时候就会出现依赖包嵌套，嵌套太多层就导致结构非常混乱，在npm3的时候，采用扁平依赖树来解决，所以我们的项目下只有node_modules和其他的包，但是用这个包就必须遍历所有依赖包再生成依赖树，所以npm下载非常耗时

cnpm
因为国内使用npm下载太慢了，所以淘宝提供了镜像cnpm访问

cnpm坑点：npm有packge-lock.json是用来锁定安装的包的版本号，但是cnpm不受packge-lock.json的限制，cnpm只根据packge.json来下载安装包

yarn
yarn是由Google和Facebook等公司开发出来的新的包管理工具，由于npm有以下缺点：

下载速度慢
安装速度慢
下载版本不一致
所以yarn针对这些缺点：

通过并行下载，提高下载的速度
通过yarn.lock来保存包之间的依赖关系，保证包的版本一致
通过yarn.lock来保存依赖关系，下一次安装更快
支持离线下载
npm下载都会有本地缓存，但是npm需要联网才能从缓存中获取，而yarn可以不联网就离线下载
npm5也支持了包的版本一致，使用了packge-lock.json

pnpm
pnpm的下载速度甚至超过了yarn和npm，它使用硬链接和符号链接避免复制所有本地源文件，同时也继承了yarn的优点，支持离线下载和包的版本一致问题

---

全局安装
npm install pnpm -g
1
设置源
pnpm config get registry // 查看源
pnpm config set registry http://registry.npm.taobao.org // 切换淘宝源
1
2
使用
pnpm install 包
pnpm i 包
pnpm add 包    // -S  默认写入dependencies
pnpm add -D    // -D devDependencies
pnpm add -g    // 全局安装
pnpm remove 包         //移除包
pnpm remove 包 --global   //移除全局包
pnpm up                //更新所有依赖项
pnpm upgrade 包        //更新包
pnpm upgrade 包 --global   //更新全局包
设置存储路径：pnpm config set store-dir /path/to/.pnpm-store
————————————————

npm install 依赖包名称@latest -D

###### 查看最新版本

npm info 依赖包名称 version（查看当前最新版本）
npm info 依赖包名称 versions（查看所有版本)
npm view 依赖包名称 version（查看当前最新版本）
npm view 依赖包名称 versions（查看所有版本）
