# Integrating Angular and Electron

- [Integrating Angular and Electron](#integrating-angular-and-electron)
  - [Structure](#structure)
  - [Create the Angular App](#create-the-angular-app)
  - [Create the Electron App](#create-the-electron-app)
  - [Integrating the Angluar App inside the Electron App](#integrating-the-angluar-app-inside-the-electron-app)
  - [Running the app](#running-the-app)
  - [Get access to electron APIs from within your Angular app](#get-access-to-electron-apis-from-within-your-angular-app)
  - [To do:](#to-do)

## Structure

I'm using the following folder structure

```
project/
 angular/
 electron/
```

## Create the Angular App

Inside the `project` folder:  
Creating a new angular project

```
# Create a new workspace and an initial Angular app
ng new angular
# Go into the app
cd angular
# Run the app
ng serve --open
```

## Create the Electron App

Inside the `project` folder:  
Cloning the _electron-quick-start-typescript_ from GitHub to use it as boilerplate:
For more details click [here](https://github.com/electron/electron-quick-start-typescript).

```
# Clone this repository
git clone https://github.com/electron/electron-quick-start-typescript electron
# Go into the repository
cd electron
# Install dependencies
npm install
# Run the app
npm start
```

You can savely remove the `project/electron/.git` folder.

## Integrating the Angluar App inside the Electron App

Now you should have folders like this:

```
project/
 angular/
 electron/
```

Open the `project/angular/package.json` and add two scripts:

```json
...
"scripts": {
    "electron:build": "ng build --baseHref=./ --outputPath=../electron/dist/ng",
    "electron:watch": "ng build --watch --baseHref=./ --outputPath=../electron/dist/ng"
  }
...
```

The `electron:build` script will build the Angular app inside an app folder inside our Electron folder `project/electron/dist/ng`.  
The `electron:wath` script will do the same as `electron:build` and will rebuild on every change.  
`--baseHref = ./` is setting the base url for the application being built to `./` as it is needed for electron.

Now we have to tell the electron app to load the angular app. To do this open the `project/electron/src/main.ts` and change

```typescript
mainWindow.loadFile(path.join(__dirname, '../index.html'));
```

to

```typescript
mainWindow.loadFile(path.join(__dirname, 'ng/index.html'));
```

Now the build output from the angular app is used by the electron app.

## Running the app

- Open a terminal and move to the Angular folder (`project/angular`) and run  
  `npm run electron:watch`  
  to build the Angular app and rebuild on every change.
- Open a second terminal and move to the electron folder (`project/electron`) and run  
  `npm start` to start the electron application.

## Get access to electron APIs from within your Angular app

I found [this blogpost](https://thorsten-hans.com/integrating-angular-and-electron-using-ngx-electron-9c36affca25e) from [Thorsten Hans](https://thorsten-hans.com). He is describing his package `ngx-electron` which wraps all the electron API’s exposed to the renderer process into a single `ElectronService`.

To install the package run npm install inside the `project/angular` folder:

```
npm install electron --save-dev
npm install ngx-electron --save
```

You'll find more information about `ngx-electron`

- in the blog post: https://thorsten-hans.com/integrating-angular-and-electron-using-ngx-electron-9c36affca25e
- on GitHub: https://github.com/ThorstenHans/ngx-electron/

## To do:

- `npm install` inside the `project/electron` folder found 1 critical severity vulnerability. `npm audit fix --force` crashes the app.
- update electron
- Add packager to tutorial
