# React electron typescript boilerplate

<h1 align="center">
<span><img src="https://cursosdedesarrollo.com/wp-content/uploads/2019/11/react.svg" width="150px"></span>
<span><img src="https://lineadecodigo.com/wp-content/uploads/2017/08/typescript.png" width="150px"></span>
<span><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Electron_Software_Framework_Logo.svg/1200px-Electron_Software_Framework_Logo.svg.png" width="150px"></span>
</h1>


This is a boilerplate that have a react app with typescript into electronjs.

## Notes:
1. This was made to init a project, it has not been properly tested.
2. Even if the react project was made with create-react-app template typescript, it have not support for css modules for types, you need to create a **\<css-file-name\>.module.css.d.ts** for each css module you have.
3. The build for electron app it has not been tested.

## How this boilerplate was made.

### React app.

The react app was made with create-react-app, with typescript template.
`npx create-react-app ./ --template typescript`

the `./` is for creating the app in the folder where you are, i usually create the folder first and then the app in that folder.

### Electron.

We added electron following this [article](https://medium.com/@xagustin93/a%C3%B1adiendo-react-a-una-aplicaci%C3%B3n-de-electron-ab3df35f48fd).

Here are the step to add electron:

#### 1) add electron dependencies and some usefull libraries.
`yarn add electron electron-builder wait-on concurrently --dev`

#### 2) Create /public/electron.js file

```javascript
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

const path = require('path');
const isDev = process.env.NODE_ENV === 'production' ? false : true;

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({width: 900, height: 680});
  mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
  if (isDev) {
    // Open the DevTools.
    //BrowserWindow.addDevToolsExtension('<location to your react chrome extension>');
    mainWindow.webContents.openDevTools();
  }
  mainWindow.on('closed', () => mainWindow = null);
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```

#### 3) Add this script in package.json.
`"electron-dev": "concurrently \"yarn start\" \"wait-on http://localhost:3000 && electron .\""`

#### 4) Add main in package.json.
`"main": "public/electron.js"`

then you can run `yarn electron-dev` to test the app.