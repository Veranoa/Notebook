# Electron

#### 用途
使用 JavaScript、HTML 和 CSS 构建桌面应用程序的框架。通过将 Chromium 和 Node.js 嵌入到其二进制文件中，Electron 维护一个 JavaScript 代码库并创建可在 Windows、macOS 和 Linux 上运行的跨平台应用程序 

#### 安装
npm i --save-dev electron

npx electron .

### 使用Electron打包

```js
// main.js

//for macOS

// Modules to control application life and create native browser window
const { app, BrowserWindow } = require('electron')
const path = require('path')

const createWindow = () => {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  // and load the index.html of the app.
  mainWindow.loadFile('index.html')

  // Open the DevTools.
  // mainWindow.webContents.openDevTools()
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.whenReady().then(() => {
  createWindow()

  app.on('activate', () => {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})

// Quit when all windows are closed, except on macOS. There, it's common
// for applications and their menu bar to stay active until the user quits
// explicitly with Cmd + Q.
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit()
})

// In this file you can include the rest of your app's specific main process
// code. You can also put them in separate files and require them here.
```
1. 在package.json配置entry point
```json
"main":"main.js",
```

2. 在package.json配置author
```json
"author":"toutou",
```

3. 将electron安装到应用程序的devDependencies
```
npm install --save-dev electron
```

4. 在package.json的scripts配置Electron（需要与start区分）
```json
"electron-start": "electron ."
```

5. 在根目录创建main文件main.tsx并配置主进程
```tsx
const { app, BrowserWindow, nativeImage } = require('electron');
```

6. 在main.tsx添加creatWindow()加载本地 HTML 文件或远程 URL
```tsx
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600
  })

  win.loadFile('index.html')
  // win.loadURL('http://localhost:3000/');
}
```
7. 在main.tsx可配置窗口特性，例：
```tsx
webPreferences: { // 网页功能设置
  nodeIntegration: true, // 是否启用node集成 渲染进程的内容有访问node的能力
  webviewTag: true, // 是否使用<webview>标签 在一个独立的 frame 和进程里显示外部 web 内容
  webSecurity: false, // 禁用同源策略
  nodeIntegrationInSubFrames: true // 是否允许在子页面(iframe)或子窗口(child window)中集成Node.js
    }
```
8. 管理窗口
当所有窗口关闭时退出应用程序（Windows 和 Linux ）
```tsx
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit()
})
```
   如果没有打开窗口，则打开一个窗口（macOS）
```tsx
app.whenReady().then(() => {
  createWindow()

  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})
```

9.   使用预加载
将 Electron 的版本号及其依赖项打印到网页上
创建preload.js，运行replaceText辅助函数将版本号加入HTML文档
```js
window.addEventListener('DOMContentLoaded', () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector)
    if (element) element.innerText = text
  }

  for (const dependency of ['chrome', 'node', 'electron']) {
    replaceText(`${dependency}-version`, process.versions[dependency])
  }
})
```

在main.tsx将此脚本附加到渲染器进程，将预加载脚本的路径传递给webPreferences.preload现有BrowserWindow构造函数中的选项。
```tsx
const { app, BrowserWindow } = require('electron')
// include the Node.js 'path' module at the top of your file
const path = require('path')

// modify your existing createWindow() function
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
      //将多个路径段连接在一起，创建一个适用于所有平台的组合路径字符串。
    }
  })

  win.loadFile('index.html')
}
// ...
```

