# 一小时快速上手Electron
[B站张天禹《一小时快速上手Electron》](https://www.bilibili.com/video/BV1sE421N7M5/?spm_id_from=333.337.search-card.all.click&vd_source=e2167cff24472e54e910b2833b7a3006)课程笔记

## 一、什么是 Electron？ 

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737022062414-b2b08a2f-10b2-4182-ae47-d41d05b5cdc9.png)

Electron 是⼀个跨平台桌⾯应⽤开发框架，开发者可以使⽤：HTML、CSS、JavaScript 等 

Web 技术来构建桌⾯应⽤程序，它的本质是结合了 Chromium 和 Node.js，现在⼴泛⽤于桌⾯应 

⽤程序开发，例如这写桌⾯应⽤都⽤到了 Electron 技术： 

- VisualStudioCode 
- GitHubDesktop 
- 1Password 
- 新版 QQ

## 二、Electron 的优势 

1. 可跨平台：同⼀套代码可以构建出能在：Windows、macOS、Linux 上运⾏的应⽤程序。
2. 上⼿容易：使⽤ Web 技术就可以轻松完成开发桌⾯应⽤程序。
3. 底层权限：允许应⽤程序访问⽂件系统、操作系统等底层功能，从⽽实现复杂的系统交互。
4. 社区⽀持：拥有⼀个庞⼤且活跃的社区，开发者可以轻松找到⽂档、教程和开源库。

## 三、Electron 技术架构 

### (一) 技术架构 

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737022376919-3de2b464-58e3-44dd-9b89-0bf1872c2c13.png)

### (二) 进程模型 

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737022418096-cf4a0d1e-7942-4d28-9008-8867bb24dc12.png)

## 四、搭建⼀个工程

- 初始化—个包， 并提填写好  package.json 中的必要信息及启动命令。

```json
{
  "name": "test",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "start": "electron ." //start命令⽤于启动整个应⽤
  },
  "author": "tianyu", //为后续能顺利打包，此处要写明作者。
  "license": "ISC",
  "description": "this is a electron demo", //为后续能顺利打包，此处要编写描述。
}
```

- 安装 electron 作为开发依赖。

```bash
npm i electron -D
```

- 在 main.js 中编写代码，创建⼀个基本窗⼝

```javascript
/*
 main.js运⾏在应⽤的主进程上，⽆法访问Web相关API，主要负责：控制⽣命周期、显示界⾯、
控制渲染进程等其他操作。
*/
const { app, BrowserWindow } = require('electron')
// ⽤于创建窗⼝
function createWindow() {
  const win = new BrowserWindow({
    width: 800, // 窗⼝宽度
    height: 600, // 窗⼝⾼度
    autoHideMenuBar: true, // ⾃动隐藏菜单栏
    alwaysOnTop: true, // 置顶
    x: 0, // 窗⼝位置x坐标
    y: 0 // 窗⼝位置y坐标
  })
  // 加载⼀个远程⻚⾯
  win.loadURL('http://www.atguigu.com')
}
// 当app准备好后，执⾏createWindow创建窗⼝
app.on('ready',()=>{
  createWindow()
})
```

关于 BrowserWindow 的更多配置项，请参考：[BrowserWindow实例属性](https://www.electronjs.org/zh/docs/latest/api/base-window#实例属性)

- 启动应⽤查看效果

```bash
npm start
```

- 效果如下：

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737022645278-b3b0576d-34fc-470a-b35e-ce4d6f3d6724.png)

## 五、加载本地页面

- 创建  pages/index.html 编写内容：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>index</title>
  </head>
  <body>
    <h1>你好啊！</h1>
  </body>
</html>
```

- 修改 mian.js 加载本地⻚⾯

```javascript
// 加载⼀个本地⻚⾯
win.loadFile('./pages/index.html')
```

- 此时开发者⼯具会报出⼀个安全警告，需要修改 index.html ，配置 CSP(Content-Security-Policy)

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; st
  yle-src 'self' 'unsafe-inline'; img-src 'self' data:;">
```



<details class="lake-collapse"><summary id="uffeb47cf"><span class="ne-text" style="color: rgb(38,38,38); background-color: #FBDE28; font-size: 16px">上述配置的说明 </span></summary><p id="ub3940503" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">1. </span></strong><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">default-src 'self' </span></strong></p><p id="u537307ea" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">default-src </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：配置加载策略，适⽤于所有未在其它指令中明确指定的资源类型。 </span></p><p id="u85c4ab64" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">self </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：仅允许从同源的资源加载，禁⽌从不受信任的外部来源加载，提⾼安全性。 </span></p><p id="u97e9e1e1" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">2. </span></strong><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">style-src 'self' 'unsafe-inline' </span></strong></p><p id="u20560d47" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">style-src </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：指定样式表（CSS）的加载策略。 </span></p><p id="u81ac5d08" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">self </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：仅允许从同源的资源加载，禁⽌从不受信任的外部来源加载，提⾼安全性。 </span></p><p id="u141e8bc3" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">unsafe-inline </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：允许在HTML⽂档内使⽤内联样式。 </span></p><p id="u763c8d4b" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">3. </span></strong><strong><span class="ne-text" style="color: rgb(12,104,202); font-size: 16px">img-src 'self' data: </span></strong></p><p id="u58786afa" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">img-src </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：指定图像资源的加载策略。 </span></p><p id="u716a2c16" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">self </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：表示仅允许从同源加载图像。 </span></p><p id="u76bf8d2c" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">data: </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">：允许使⽤ </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">data: URI </span><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">来嵌⼊图像。这种URI模式允许将图像数据直接嵌 </span></p><p id="ud25792ed" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">⼊到HTML或CSS中，⽽不是通过外部链接引⽤。 </span></p><p id="u7d315120" class="ne-p" style="margin: 0; padding: 0; min-height: 24px; margin-left: 2em"><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">关于 CSP 的详细说明请参考：</span><a href="https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy" data-href="https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy" target="_blank" class="ne-link"><span class="ne-text" style="font-size: 16px">MDN-Content-Security-Policy</span></a><span class="ne-text" style="color: rgb(38,38,38); font-size: 16px">、</span><a href="https://www.electronjs.org/docs/latest/tutorial/security" data-href="https://www.electronjs.org/docs/latest/tutorial/security" target="_blank" class="ne-link"><span class="ne-text" style="font-size: 16px">Electron Security</span></a></p></details>

## 六、 完善窗口行为

\1. Windows 和 Linux 平台窗⼝特点是：关闭所有窗⼝时退出应⽤。

```javascript
// 当所有窗⼝都关闭时
app.on('window-all-closed', () => {
  // 如果所处平台不是mac(darwin)，则退出应⽤。
  if (process.platform !== 'darwin') app.quit()
})
```

\2. mac 应⽤即使在没有打开任何窗⼝的情况下也继续运⾏，并且在没有窗⼝可⽤的情况下激活 应⽤时会打开新的窗⼝。

```javascript
// 当app准备好后，执⾏createWindow创建窗⼝
app.on('ready',()=>{
  createWindow()
  // 当应⽤被激活时
  app.on('activate', () => {
    //如果当前应⽤没有窗⼝，则创建⼀个新的窗⼝
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})
```

## 七、配置自动重启

1. 安装 Nodemon

```bash
npm i nodemo
```

1. 修改 package.json 命令

```json
"scripts": {
  "start": "nodemon --exec electron ."
},
```

1. 配置 nodemon.json 规则

```json
{
  "ignore": [
    "node_modules",
    "dist"
  ],
  "restartable": "r",
  "watch": ["*.*"],
  "ext": "html,js,css"
}
```

配置好以后，当代码修改后，应⽤就会⾃动重启了。

## 八、主进程与渲染进程 

下图是 Chrome 浏览器的程序架构，图来⾃于[Chrome 漫画](https://www.google.com/googlebooks/chrome/)。

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737023837567-d8b86a2b-39e5-4668-a37c-1c1783181d62.png)

Electron 应⽤的结构与上图⾮常相似，在 Electron 中主要控制两类进程：主进程、渲染器进程。 

### (一) 主进程 

每个 Electron 应⽤都有⼀个单⼀的主进程，作为应⽤程序的⼊⼝点。 主进程在 Node.js 环境中运行，它具有 require 模块和使⽤所有 Node.js API 的能⼒，主进程的核⼼就是：使用BrowserWindow 来创建和管理窗口。

### (二) 渲染进程 

每个 BrowserWindow 实例都对应⼀个单独的渲染器进程，运⾏在渲染器进程中的代码，必须遵 守⽹⻚标准，这也就意味着：浏览器进程无权直接访问 require 或使用任何Node.js的API。



问题产⽣：处于渲染器进程的⽤户界⾯，该怎样才与 Node.js 和 Electron 的原⽣桌⾯功能进⾏ 

交互呢？

## 九、Preload 脚本 

预加载（Preload）脚本是运⾏在渲染进程中的， 但它是在⽹⻚内容加载之前执行的，这意味着它具有比普通渲染器代码更⾼的权限，可以访问 Node.js 的 API，同时又可以与网页内容进行安全的交互。 



简单说：它是 Node.js 和 Web API 的桥梁，Preload 脚本可以安全地将部分 Node.js 功能暴露 给网页，从而减少安全⻛险。



需求：点击按钮后，在网页呈现当前的 Node 版本。



具体文件结构与编码如下：

1. 创建预加载脚本 preload.js ，内容如下：

```javascript
const {contextBridge} = require('electron')
// 暴露数据给渲染进程
contextBridge.exposeInMainWorld('myAPI',{
  n:666,
  version:process.version
})
```

1. 在主线程中引入 preload.js

```javascript
const win = new BrowserWindow({
 /*******/
 webPreferences:{
 preload:path.resolve(__dirname,'./preload.js')
 }
 /*******/
})
```

1. 在 html 网页中编写对应按钮，并创建专⻔编写⽹⻚脚本的 render.js ，随后引⼊。

```html
<body>
  <h1>你好啊！</h1>
  <button id="btn">在⽤户的D盘创建⼀个hello.txt</button>
  <script type="text/javascript" src="./render.js"></script>
</body>
```

1. 在渲染进程中使⽤ version

```javascript
btn.addEventListener('click',()=>{
  console.log(myAPI.version)
  document.body.innerHTML += `<h2>${myAPI.version}</h2>`
})
```

1. 整体⽂件结构如下：

![img](https://cdn.nlark.com/yuque/0/2025/png/1789339/1737024423601-c7a6225e-b2e9-45b9-a688-2f2d16f42f71.png)

## 十、进程通信（IPC） 

值得注意的是： 

上⽂中的 preload.js ，⽆法使⽤全部 Node 的 API ，⽐如：不能使⽤ Node 中的 fs

模块，但主进程（ main.js ）是可以的，这时就需要进程通信了。简单说：要让 preload.js 通知main.js 去调⽤ fs 模块去⼲活。

关于 Electron 进程通信，我们要知道：

- IPC 全称为： InterProcess Communication ，即：进程通信。
- IPC 是 Electron 中最为核心的内容，它是从 UI 调⽤原⽣ API 的唯⼀方法！
- Electron 中，主要使⽤ ipcMain 和 ipcRenderer 来定义“通道”，进行进程通信。

### (一) 渲染进程➡️主进程（单向）

概述：在渲染器进程中 ipcRenderer.send 发送消息，在主进程中使⽤ ipcMain.on 接收消息。 

常⽤于： 在web中调用主进程的API，例如下⾯的这个需求：

需求：点击按钮后，在⽤户的 D 盘创建⼀个 **hello.txt** ⽂件，⽂件内容来⾃于⽤户输⼊。 

1. ⻚⾯中添加相关元素， render.js 中添加对应脚本

```html
<input id="content" type="text"><br><br>
<button id="btn">在⽤户的D盘创建⼀个hello.txt</button>
const btn = document.getElementById('btn')
const content = document.getElementById('content')
btn.addEventListener('click',()=>{
  console.log(content.value)
  myAPI.saveFile(content.value)
})
```

1. preload.js 中使⽤ **ipcRenderer.send('**信道**',**参数**)** 发送消息，与主进程通信。

```javascript
const {contextBridge,ipcRenderer} = require('electron')
contextBridge.exposeInMainWorld('myAPI',{
  /*******/
  saveFile(str){
    // 渲染进程给主进程发送⼀个消息
    ipcRenderer.send('create-file',str)
  }
})
```

1. 主进程中，在加载⻚⾯之前，使⽤ **ipcMain.on('**信道**',**回调**)** 配置对应回调函数，接收 消息。

```javascript
// ⽤于创建窗⼝
function createWindow() {
  /**********/
  // 主进程注册对应回调
  ipcMain.on('create-file',createFile)
  // 加载⼀个本地⻚⾯
  win.loadFile(path.resolve(__dirname,'./pages/index.html'))
}
//创建⽂件
function createFile(event,data){
  fs.writeFileSync('D:/hello.txt',data)
}
```

### (二) 渲染进程↔主进程（双向） 

概述：渲染进程通过ipcRenderer.invoke 发送消息，主进程使⽤ipcMain.handle 接收并处理消息。

备注： ipcRender.invoke 的返回值是 Promise 实例



常⽤于： **从渲染器进程调用主进程方法并等待结果**，例如下⾯的这个需求：

需求：点击按钮从 D 盘读取 **hello.txt** 中的内容，并将结果呈现在⻚⾯上。

1.  ⻚⾯中添加相关元素， render.js 中添加对应脚本

```html
<button id="btn">读取⽤户D盘的hello.txt</button>
const btn = document.getElementById('btn')
btn.addEventListener('click',async()=>{
  let data =
    document.body.innerHTML += `<h2>${data}</h2>`
})
```

1. preload.js 中使⽤ **ipcRenderer.invoke('**信道**',**参数**)** 发送消息，与主进程通信。

```javascript
const {contextBridge,ipcRenderer} = require('electron')
contextBridge.exposeInMainWorld('myAPI',{
  /*******/
  readFile (path){
    return ipcRenderer.invoke('read-file')
  }
})
```

1. 主进程中，在加载⻚⾯之前，使⽤ **ipcMain.handle('**信道**',**回调**)** 接收消息，并配置回 调函数。

```javascript
// ⽤于创建窗⼝
function createWindow() {
  /**********/
  // 主进程注册对应回调
  ipcMain.handle('read-file',readFile)
  // 加载⼀个本地⻚⾯
  win.loadFile(path.resolve(__dirname,'./pages/index.html'))
}
//读取⽂件
function readFile(event,path){
  return fs.readFileSync(path).toString()
}
```

### (三) 主进程到➡️渲染进程 

概述：主进程使⽤ win.webContents.send 发送消息，渲染进程通过ipcRenderer.on 处理消息， 

常⽤于： **从主进程主动发送信息给渲染进程**，例如下⾯的这个需求：



需求：应⽤加载 6 秒钟后，主动给渲染进程发送⼀个消息，内容是：你好啊！

1. ⻚⾯中添加相关元素， render.js 中添加对应脚本

```javascript
window.onload = ()=>{
  myAPI.getMessage(logMessage)
}
function logMessage(event,str){
  console.log(event,str)
}
```

1. preload.js 中使⽤ **ipcRenderer.****on** **('**信道**',**回调**)** 接收消息，并配置回调函数。

```javascript
const {contextBridge,ipcRenderer} = require('electron')
contextBridge.exposeInMainWorld('myAPI',{
 /*******/
 getMessage: (callback) => {
 return ipcRenderer.on('message', callback);
 }
})
```

1. 主进程中，在合适的时候，使⽤ **win.webContents.send('**信道**',**数据**)** 发送消息。

```javascript
// ⽤于创建窗⼝
function createWindow() {
  /**********/
  // 加载⼀个本地⻚⾯
  win.loadFile(path.resolve(__dirname,'./pages/index.html'))
  // 创建⼀个定时器
  setTimeout(() => {
    win.webContents.send('message','你好啊！')
  }, 6000);
}
```

## 十一、打包应用

使⽤ electron-builder 打包应⽤

1. 安装 electron-builder ：

```bash
npm install electron-builder -D
```

1. 在 package.json 中进⾏相关配置，具体配置如下：

备注：json ⽂件不⽀持注释，使⽤时请去掉所有注释

```json
{
  "name": "video-tools", // 应⽤程序的名称
  "version": "1.0.0", // 应⽤程序的版本
  "main": "main.js", // 应⽤程序的⼊⼝⽂件
  "scripts": {
    "start": "electron .", // 使⽤ `electron .` 命令启动应⽤程序
    "build": "electron-builder" // 使⽤ `electron-builder` 打包应⽤程序，⽣成
    安装包
  },
  "build": {
    "appId": "com.atguigu.video", // 应⽤程序的唯⼀标识符
    // 打包windows平台安装包的具体配置
    "win": {
      "icon":"./logo.ico", //应⽤图标
      "target": [
        {
          "target": "nsis", // 指定使⽤ NSIS 作为安装程序格式
          "arch": ["x64"] // ⽣成 64 位安装包
        }
      ]
    },
    "nsis": {
      "oneClick": false, // 设置为 `false` 使安装程序显示安装向导界⾯，⽽不是⼀
      键安装
      "perMachine": true, // 允许每台机器安装⼀次，⽽不是每个⽤户都安装
      "allowToChangeInstallationDirectory": true // 允许⽤户在安装过程中选择
      安装⽬录
    }
  },
  "devDependencies": {
    "electron": "^30.0.0", // 开发依赖中的 Electron 版本
    "electron-builder": "^24.13.3" // 开发依赖中的 `electron-builder` 版本
  },
  "author": "tianyu", // 作者信息
  "license": "ISC", // 许可证信息
  "description": "A video processing program based on Electron" // 应⽤程
  序的描述
}
```

1. 执⾏打包命令

```bash
npm run build
```

## 十二、electron-vite

electron-vite 是⼀个新型构建⼯具，旨在为 Electron 提供更快、更精简的体验。主要由五部分 

组成： 

● ⼀套构建指令，它使⽤ Vite 打包你的代码，并且它能够处理 Electron 的独特环境，包括 Node.js 和浏览器环境。 

● 集中配置主进程、渲染器和预加载脚本的 Vite 配置，并针对 Electron 的独特环境进⾏预配 置。 

● 为渲染器提供快速模块热替换（HMR）⽀持，为主进程和预加载脚本提供热重载⽀持，极⼤ 地提⾼了开发效率。 

● 优化 Electron 主进程资源处理。 

● 使⽤ V8 字节码保护源代码。 electron-vite 快速、简单且功能强⼤，旨在开箱即⽤。 

**官⽹地址：****https://cn-evite.netlify.app/**