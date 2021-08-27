# [electron](https://www.electronjs.org/)
技术分享之 Electron

## 介绍
Electron（原名为 Atom Shell）是 GitHub 开发的一个开源框架。它通过使用 Node.js（作为后端）和 Chromium 的渲染引擎（作为前端）完成跨平台的桌面 GUI 应用程序的开发。Electron 现已被多个开源 Web 应用程序用于前端与后端的开发，著名项目包括 GitHub 的 Atom 和微软的 Visual Studio Code。[【维基百科】](https://zh.wikipedia.org/wiki/Electron)

## 行业应用
1. 浏览器
2. 代码编辑器：VSCode / Atom
3. 游戏开发工具：Cocos Creator
4. 及时通讯工具：Facebook Messenger / Slack

## 传统客户端方案
1. C++ / Qt
2. .NET / Java

## 一些问题
Q1: 为什么越来越多客户端软件使用 Web 前端技术开发？

Q2: 为什么 Web 前端技术开发 UI 界面效率高？

## 同类框架
1. [NW.js](https://nwjs.io/)（前身：node-webkit）
2. [Cordova](https://cordova.apache.org/)（前身：PhoneGap）

## [Electron vs NW.js](https://www.electronjs.org/docs/development/electron-vs-nwjs)
|| Electron | NW.js |
|----|----|----|
| 入口 | Javascript 脚本，类似 Node.js 运行时。| HTML 页面，直接在浏览器打开。|
| Node 集成 | 集成操作系统模块 | 给 Chromium 打补丁 |
| Javascript 上下文 | 不区分上下文 | 区分 Node 上下文和 Web 上下文 |
| 向后兼容 | Windows 7 | Windows XP |
| 生态 | 更大的社区，更多应用，更多模块。| 更多 Chrome API 支持 |

## 主要功能
1. 本地文件读写（通过 Node.js [fs 模块](http://nodejs.cn/api/fs.html)）
2. 可定制的[浏览器窗口](https://www.electronjs.org/docs/api/browser-window#new-browserwindowoptions)

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

3. [打印功能](https://www.electronjs.org/docs/api/web-contents#contentsprintoptions-callback)

  Q: 打印网页没有数据，会是什么原因？

4. [窗口通信](https://www.electronjs.org/docs/api/web-contents#contentspostmessagechannel-message-transfer)

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

5. [系统通知](https://www.electronjs.org/docs/api/notification)

## 其它优点
1. 最新的 Chromium 内核，没有兼容性问题。
2. 远程网站更新方便
3. 应用能够同时运行在浏览器和客户端

## 公司相关项目
1. 华强北智慧警亭：双屏联动、TRTC 实时音视频、系统通知、打印功能
2. 纽斯达：客户要求、打印功能
3. 深圳供电局大屏：客户要求、系统配置

## 拓展
1. 前端项目的本地化运行

## 写在最后
Any application that can be written in JavaScript, will eventually be written in JavaScript.

-- Jeff Atwood, 2007