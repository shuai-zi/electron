# Electron 入门
======================================
## 开发环境
为了打造一个Electron桌面程序的开发环境，你只需要安装好的Node.js、npm、一个顺手的代码编辑器以及对你的操作系统命令行客户端的基本了解。

## 打造你第一个 Electron 应用
从开发的角度来看, Electron application 本质上是一个 Node. js 应用程序。 应用启动的入口是一个与 Node.js 模块相同的 package.json 文件。 一个最基本的 Electron 应用一般来说会有如下的目录结构：
```
your-app/
  ├── package.json
  ├── main.js
  └── index.html
```
为你的新Electron应用创建一个新的空文件夹。 打开你的命令行工具，然后从该文件夹运行npm init ，将会创建一个package.json文件。
```
npm init
```
## 安装[Electron](https://electronjs.org/docs)
1. 在您的app所在文件夹中运行下面的命令：
```
npm install --save-dev electron
```
2. 在你的项目文件夹下面创建main.js文件，main.js代码如下：
```
const {app, BrowserWindow} = require('electron')
  const path = require('path')
  const url = require('url')
  
  // 保持一个对于 window 对象的全局引用，如果你不这样做，
  // 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
  let win
  
  function createWindow () {
    // 创建浏览器窗口。
    win = new BrowserWindow({width: 800, height: 600})
  
    // 然后加载应用的 index.html。
    win.loadURL(url.format({
      pathname: path.join(__dirname, 'index.html'),
      protocol: 'file:',
      slashes: true
    }))
  
    // 打开开发者工具。
    win.webContents.openDevTools()
  
    // 当 window 被关闭，这个事件会被触发。
    win.on('closed', () => {
      // 取消引用 window 对象，如果你的应用支持多窗口的话，
      // 通常会把多个 window 对象存放在一个数组里面，
      // 与此同时，你应该删除相应的元素。
      win = null
    })
  }
  
  // Electron 会在初始化后并准备
  // 创建浏览器窗口时，调用这个函数。
  // 部分 API 在 ready 事件触发后才能使用。
  app.on('ready', createWindow)
  
  // 当全部窗口关闭时退出。
  app.on('window-all-closed', () => {
    // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
    // 否则绝大部分应用及其菜单栏会保持激活。
    if (process.platform !== 'darwin') {
      app.quit()
    }
  })
  
  app.on('activate', () => {
    // 在macOS上，当单击dock图标并且没有其他窗口打开时，
    // 通常在应用程序中重新创建一个窗口。
    if (win === null) {
      createWindow()
    }
  })
  
  // 在这个文件中，你可以续写应用剩下主进程代码。
  // 也可以拆分成几个文件，然后用 require 导入。
```
3. 最后，创建你想展示的 index.html：
```
<!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Hello World!</title>
    </head>
    <body>
      <h1>Hello World!</h1>
      We are using node <script>document.write(process.versions.node)</script>,
      Chrome <script>document.write(process.versions.chrome)</script>,
      and Electron <script>document.write(process.versions.electron)</script>.
    </body>
  </html>
```
## 启动你的应用
在创建并初始化完成 main.js、 index.html和package.json之后，您就可以在当前工程的根目录执行 npm start 命令来启动刚刚编写好的Electron程序了。
我们可以看到应用已经完美的跑了起来，我们开启了devTools因此，我们的应用应该是这样的。
![Electron](https://raw.githubusercontent.com/shuai-zi/electron/master/myapp.png)
## 尝试此例
###### tip：您可能需要预先安装Git环境
```
# 克隆这仓库
  $ git clone https://github.com/electron/electron-quick-start
  # 进入仓库
  $ cd electron-quick-start
  # 安装依赖库
  $ npm install
  # 运行应用
  $ npm start
```


# 打包你的Electron项目
需要是使用到electron-packager,所以先安装：
```
npm install --save-dev electron-packager
```
安装完成后我们在命令行输入如下格式的命令：
electron-packager <应用目录> <应用名称> <打包平台> --out <输出目录> <架构> <应用版本>
```
electron-packager . HelloWorld --win --out ../HelloWorldApp --arch=x64 --version=0.0.1 --electron-version=1.4.13
```

执行完毕后，看到父级目录下已经产生了我们希望看到的应用文件夹。
