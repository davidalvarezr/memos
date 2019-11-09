# Electron

## Getting started

Put this `main.js` file on the root folder of your app:
```js
const {app, BrowserWindow} = require('electron')
const url = require("url");
const path = require("path");

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  mainWindow.loadURL(
    url.format({
      pathname: path.join(__dirname, `/dist/<proj_name>/index.html`), // <-- path to the entry point once built
      protocol: "file:",
      slashes: true
    })
  );
  // Open the DevTools.
  mainWindow.webContents.openDevTools()

  mainWindow.on('closed', function () {
    mainWindow = null
  })
}

app.on('ready', createWindow)

app.on('window-all-closed', function () {
  if (process.platform !== 'darwin') app.quit()
})

app.on('activate', function () {
  if (mainWindow === null) createWindow()
})

```

## Run (Angular project)

In the root folder
```bash
ng build --base-href      # Build project (generally in `dist/<project_name>/`)
electron .                # Run the built project with Electron
``` 

## Build binaries on curent OS

```bash
ng build --base-href      # Build project (generally in `dist/<project_name>/`)
electron-forge make       # Let electron-forge create the lauchable program in `out/`
```
---

## Tips

### Remove toolbar

To do **before creating the browser window** 
```js
const { app, BrowserWindow, Menu } = require("electron");

Menu.setApplicationMenu(null);

// ...
```
