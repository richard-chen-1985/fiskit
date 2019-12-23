# fiskit

A front-end toolkit based on fis （基于fis的纯前端脚手架工具）

确认已正确安装`nodejs`，然后执行`npm install -g fiskit`安装本模块（如果node-sass安装有问题，可以尝试 `npm install -g fiskit --sass-binary-site=http://npm.taobao.org/mirrors/node-sass` 使用淘宝镜像）。然后在项目根目录执行：

```nohighlight
// 启动内置server作为开发时的预览服务器
fk server start

// 将项目发布到内置server根目录
fk release
  [arguments]
    --domain -D 开启CDN前缀
    --pack -p 开启打包功能

// 其它命令
fk release [debug|prod]
  debug: 输出未压缩的静态资源及vm文件，用于调试时方便查找bug，默认位置：./output/debug/[version]
  prod: 输出压缩后的静态资源及vm文件，用于发布到线上，默认位置：./output/prod/[version]
```

## 文档

快速入门、配置、插件开发以及原理等文档 [FIS官方文档](http://fis.baidu.com/fis3/docs/beginning/intro.html)

## 目录结构

```nohighlight
root
  ├ _docs // 项目文档目录
  ├ page // 页面文件
  │  ├ home
  │  │  └ home.vm
  │  ├ list
  │  │  └ list.vm
  │  ├ detail
  │  │  └ detail.vm
  ├ sass // SASS库目录
  ├ mock // 测试模拟数据目录
  ├ static // 静态资源目录
  │  ├ common // 公共文件目录
  │  │  ├ img
  │  │  ├ css
  │  │  └ lib // 非模块化js目录
  │  ├ components // 模块化组件目录
  │  │  ├ dialog
  │  │  │  ├ dialog.js
  │  │  │  └ dialog.css
  │  │  ├ jquery-validate
  │  │  │  └ jquery.validate.js
  ├ widget // 页面组件目录
  │  ├ header
  │  │  ├ header.vm
  │  │  ├ header.js
  │  │  ├ header.scss
  │  │  ├ header.mock
  │  ├ nav
  │  │  ├ nav.vm
  │  │  ├ nav.js
  │  │  ├ nav.scss
  │  │  └ nav.mock
  ├ fk-conf.js // fiskit配置文件
  ├ server.conf // 本地服务器rewrite规则
```

## 配置例子

** fk-conf.js **

```nohighlight
var config = {
  // 静态资源版本号
  version: '1.0.0',
  // 打包开关，默认为false
  // 可以手动设置为true，也可以fk release -p设置为true
  packed: false,
  // cdn域名开关，prod环境始终为true
  cdn: false,
  // cdn域名地址
  cdnUrl: 'http://cdn.xxx.com',
  // MD5后缀开关
  useHash: false,
  // fis-parser-velocity 配置
  // 具体参考：https://github.com/richardchen85/fis-parser-velocity
  velocity: {
      loader: 'require',
      macro: '/macro.vm'
  },
  // fk release dev目标路径，默认不设置
  // 如果你用非fis自带server，可以直接将代码输出到你server的根目录
  devPath: '',
  // 自定义部署目录
  deploy: {
    // fk release vm 的输出目录
    vmTo: '',
    // fk release debug|prod 的输出目录
    staticTo: ''
  },
  // fk release vm环境下，deploy的replace选项
  // 默认为false, 可跟[Object|Array]
  // 具体配置请参考https://github.com/fex-team/fis3-deploy-replace
  // e.g.
  //   { from:'a', to:'b' }
  //   or
  //   [ { from:'a', to:'b' }, { from:'a0', to: b0' } ]
  replace: false,
  // 模块化配置，如不使用模块化框架，则为false
  modules: {
      // 模块规范[amd|cmd]
      mode: 'amd',
      // 以下配置请参考fis3-hook-amd或者fis3-hook-cmd配置
      forwardDeclaration: true,
      baseUrl: '',
      paths: {}
  },
  // 打包配置，默认为null无打包配置
  // media('dev')环境只在config.packed为true时打包
  // 其它media默认打包
  // @example
  //   {
  //     '/widget/**.css': {
  //       packTo: '/widget/widget_pkg.css'
  //     }
  //   }
  package: null
};

fiskit.amount(config);
```

这里有完整的[fiskit-demo](https://github.com/fis-stuff/fiskit-startup)供参考。
