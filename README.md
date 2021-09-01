# [electron](https://www.electronjs.org/)
技术分享之 Electron

## 介绍
Electron（原名为 Atom Shell）是 GitHub 开发的一个开源框架。它通过使用 Node.js（作为后端）和 Chromium 的渲染引擎（作为前端）完成跨平台的桌面 GUI 应用程序的开发。Electron 现已被多个开源 Web 应用程序用于前端与后端的开发，著名项目包括 GitHub 的 Atom 和微软的 Visual Studio Code。[【维基百科】](https://zh.wikipedia.org/wiki/Electron)

![什么是 Electron](https://raw.githubusercontent.com/xiaoda/electron/main/res/electron.png)

## [行业应用](https://www.electronjs.org/apps)

| 领域 | 主要软件 |
|----|----|
| 浏览器 | / |
| 代码编辑器 | VSCode / Atom |
| 游戏开发工具 | Cocos Creator |
| 即时通讯工具 | Slack / Facebook Messenger |
| 办公软件 | Microsoft Teams |

## 传统客户端方案
1. C++ / Qt
2. .NET / Java

***优点：***
1. 技术方案成熟
2. 性能表现好
3. 接近底层

***一些问题：***
1. 为什么越来越多客户端软件使用 Web 前端技术开发？
2. 为什么 Web 前端技术开发 UI 界面效率高？

## 同类框架
1. [NW.js](https://nwjs.io/)（前身：node-webkit）
2. [Cordova](https://cordova.apache.org/)（前身：PhoneGap）

## [Electron vs NW.js](https://www.electronjs.org/docs/development/electron-vs-nwjs)
| | Electron | NW.js |
|----|----|----|
| 入口 | Javascript 脚本，类似 Node.js 运行时。| HTML 页面，直接在浏览器打开。|
| Node 集成 | 集成操作系统模块 | 给 Chromium 打补丁 |
| Javascript 上下文 | 不区分上下文 | 区分 Node 上下文和 Web 上下文 |
| 向后兼容 | Windows 7 | Windows XP |
| 生态 | 更大的社区，更多应用，更多模块。| 更多 Chrome API 支持 |

## 主要功能
### 1. 主进程、渲染进程和[窗口通信](https://www.electronjs.org/docs/api/web-contents#contentspostmessagechannel-message-transfer)

![Electron 进程](https://raw.githubusercontent.com/xiaoda/electron/main/res/process.png)

![Electron 进程2](https://raw.githubusercontent.com/xiaoda/electron/main/res/process2.png)

``` javascript
// Main process
const { port1, port2 } = new MessageChannelMain()
webContents.postMessage('port', { message: 'hello' }, [port1])

// Renderer process
ipcRenderer.on('port', (e, msg) => {
  const [port] = e.ports
  // ...
})
```

Q1: 网页间通信有哪些方式？

Q2: 由后端支持的纯前端功能接口如何设计？

### 2. 可定制的[浏览器窗口](https://www.electronjs.org/docs/api/browser-window#new-browserwindowoptions)

  ``` javascript
  const { BrowserWindow } = require('electron')
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      webSecurity: false, // 禁用同源策略
      allowRunningInsecureContent: false, // 允许加载不安全资源
      autoplayPolicy: 'no-user-gesture-required', // 允许直接播放音视频
    }
  })
  ```

Q1: 什么是浏览器策略？

Q2: [浏览器主要有哪几部分组成？](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/#The_browser_high_level_structure)

1. 用户界面 - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。
2. 浏览器引擎 - 在用户界面和呈现引擎之间传送指令。
3. 呈现引擎 - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。
4. 网络 - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。
5. 用户界面后端 - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。
6. JavaScript 解释器。用于解析和执行 JavaScript 代码。
7. 数据存储。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。

### 3. [打印功能](https://www.electronjs.org/docs/api/web-contents#contentsprintoptions-callback)

Q: 打印网页只有页面结构没有数据，可能是什么原因？

### 4. 本地文件读写（通过 Node.js [fs 模块](http://nodejs.cn/api/fs.html)）
### 5. [系统通知](https://www.electronjs.org/docs/api/notification)

## 其它优点
1. 最新的 Chromium 内核，没有兼容性问题。
2. 远程网站更新方便
3. 项目能够同时运行在浏览器和客户端

## 公司相关项目

| 项目 | 使用原因 |
|----|----|
| 华强北智慧警亭 | 双屏联动、TRTC 实时音视频、系统通知、打印功能 |
| 纽斯达 | 客户要求、打印功能 |
| 深圳供电局展示大屏 | 客户要求、系统配置 |

## 拓展 - 前端项目的本地化运行

| 类型 | 运行方式 |
|----|----|
| 常规运行 | http://localhost:8080<br>http://anchnet.com |
| 本地运行 | file:///home/index.html |

***Q:*** 为什么要本地化运行？

***A:*** 内网环境、减少环境搭建

### 改造的关键点：Webpack [output.publicPath](https://webpack.docschina.org/configuration/output/#outputpublicpath) 配置项

对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，output.publicPath 是很重要的选项。如果指定了一个错误的值，则在加载这些资源时会收到 404 错误。

此选项指定在浏览器中所引用的「此输出目录对应的公开 URL」。相对 URL(relative URL) 会被相对于 HTML 页面（或 <base> 标签）解析。相对于服务的 URL(Server-relative URL)，相对于协议的 URL(protocol-relative URL) 或绝对 URL(absolute URL) 也可是可能用到的，或者有时必须用到，例如：当将资源托管到 CDN 时。

该选项的值是以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀。因此，在多数情况下，此选项的值都会以 / 结束。

``` javascript
module.exports = {
  //...
  output: {
    path: 'build',
    publicPath: '/',
    filename: 'static/js/[name].[contenthash:8].js',
    chunkFilename: 'static/js/[name].[contenthash:8].chunk.js'
  },
}
```

### 案例：深圳供电局展示大屏
项目架构：[CRACO](https://github.com/gsoft-inc/craco) (Create React App Configuration Override) / [Creat React App](https://create-react-app.dev/)，Webpack 配置默认由 [react-scripts](https://github.com/facebook/create-react-app) 管理。craco.config.js 可用于自定义 Webpack 配置。

如需直接管理 Webpack 配置，运行脚本：[npm run eject](https://create-react-app.dev/docs/available-scripts#npm-run-eject) (react-scripts eject)。

![npm run eject](https://raw.githubusercontent.com/xiaoda/electron/main/res/eject.png)

config/paths.js
``` javascript
const publicUrlOrPath = process.env.NODE_ENV === 'development' ? '/' : './'
// const publicUrlOrPath = getPublicUrlOrPath(
//   process.env.NODE_ENV === 'development',
//   require(resolveApp('package.json')).homepage,
//   process.env.PUBLIC_URL
// );
```

config/webpack.config.js
``` javascript
{
  loader: require.resolve('file-loader'),
  exclude: [/\.(js|mjs|jsx|ts|tsx)$/, /\.html$/, /\.json$/, /\.svg$/],
  options: {
    name: 'static/media/[name].[hash:8].[ext]',
    publicPath: '../../'
  }
}
```

#### 另一个问题：同源策略报错

![CORS error](https://raw.githubusercontent.com/xiaoda/electron/main/res/cors-error.png)

#### 解决办法（本地开发）：关闭 Chrome 同源策略。

```
// Mac
open /Applications/Google\ Chrome.app --args --user-data-dir="/var/tmp/Chrome dev session" --disable-web-security

// Windows
chrome.exe --user-data-dir="C:/Chrome dev session" --disable-web-security
```

#### 解决办法（客户端）：禁用 Electron 窗口的同源策略。

``` javascript
const { BrowserWindow } = require('electron')
const win = new BrowserWindow({
  webPreferences: {
    webSecurity: false
  }
})
```

## 相关资料
1. [【译】Electron 的本质](https://segmentfault.com/a/1190000007503495)
2. [Electron使用指南](https://zhuanlan.zhihu.com/p/142147309)

## 写在最后
Any application that can be written in JavaScript, will eventually be written in JavaScript.

-- Jeff Atwood, 2007

### 解读：
1. 用合适的技术做合适的项目
2. 技术与开发者的关系