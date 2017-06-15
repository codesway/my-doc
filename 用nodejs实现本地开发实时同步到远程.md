* 我知道在本地开发是最方便的，至少IDE能发挥更强大的作用，用起来会更爽。那就抽空找个靠谱的实时同步工具吧。

* 基于开发机环境的缘故，目前基本都使用远程开发。因为开发机的环境以及一些服务从新在本地搭建一套非常的麻烦，就算run起来也不是完全一致，因为有些依赖版库也很难保持版本一致。再加上基本没有人愿意用centos做开发，所以基本达不到完全一致的环境。之前我就遇到过一个问题phalcon框架的自动注册就算不配置在mac下也是能被引入，在centos就必须要配置。也因此获得了教训

* 之前也有说过用IDE内置的自动同步，但是在本地切换分支的时候，IDE的功能并没有监控到git分支的改变，造成切换分支后内容并没有同步上去。

* 那么就解决一下到底怎么同步才能解决类似的问题



```
很多工具都能做到实时同步，今天选择的是一个npm包来做这个事，是因为很多同步工具都不支持windows，至少Windows也能装node.js。所以就选这个吧
```
1. 先把node.js 和npm都装上，开发机装过了，所以在本地装一下就好了。（现在应该是装了node默认都会装npm管理器）

2. 然后 npm install nobone-sync 这个，要注意，开发机不能安装在全局（没目录权限）。

3. 配置一下nobone-sync，还是看一起文档吧，其实我也不会配。经过文档和我的修改配置如下：
```
module.exports = {
    localDir: '/Users/MacBook/platform/project',

    // It decides the root path to upload to.
    remoteDir: '/home/liulinliang/sync_dir/',

    // It decides the root accessible path.
    rootAllowed: '/',

    host: '127.0.0.1',
    port: 8888,	//监听端口
    pattern: '**', //全部同步
    pollingInterval: 500,

    // If it is set, transfer data will be encrypted with the algorithm.
    password: null,		//不加密
    algorithm: 'aes128',  //加密方式
    onChange: function (type, path, oldPath) {
        // It can also return a promise.
        console.log('Write your custom code here')
    }
}
```
```
有几个点要注意一下   local配置的host 要设置成开发机的IP。其他的改不改都无所谓
```

```
要run起来很简单，本地开启一个进程，开发机开启一个进程，同时监听同一个端口就OK了。 看看文档说怎么用的

serv: nobone-sync -s
client: nobone-sync config.js
（也可以起一个nohup进程，这个大家随意）
这样就OK了，先来看下我的效果

```
![图片](/upload/0010000901f2328300.png)

```
每次被监听的目录有任何变化都会被实时upload到开发机，速度也是很快的


文档上还介绍了一些配置等，有排除、推送整个目录等配置
https://www.npmjs.com/package/nobone-sync
https://github.com/ysmood/nobone-sync
如果打不开，请翻墙！！！
