# Setting up Web Dev Environment



- requires node, , vscode, web browser, if you want nvm: linux/WSL

```
```
---

## Set up WSL

### Previous WSL installation 
- if you had directories that used a prior WSL installation, you may have fo fix file permission 
- DrvFs is a filesystem plugin to WSL that was designed to support interop between WSL and the Windows filesystem.
see:
- [mount/unmount](https://codeyarns.com/tech/2020-06-17-how-to-mount-and-unmount-on-wsl.html#gsc.tab=0)
  - e.g. `sudo umount /mntd/`
  - `sudo mount -t drvfs D: /mnt/d`
  - WSL: `sudo umount /mnt/c sudo mount -t drvfs C: /mnt/c -o metadata`
- [wsl fix](https://www.turek.dev/posts/fix-wsl-file-permissions/),
- [microsoft's chmod/chown improvements](https://devblogs.microsoft.com/commandline/chmod-chown-wsl-improvements/),
- [fmask/umask](https://askubuntu.com/questions/429848/dmask-and-fmask-mount-options)
  - It works as the normal octal permissions but subtracted from 7, and use the absolute value. for instance if you want to set the permissions to 0777 you will need to set it 0000 in the umask (e.g. umask=0000), if you want to set it to 0755 you will set it to 0022 :


### install WSL [src](https://docs.microsoft.com/en-us/windows/wsl/install)

install wsl: `wsl --install -d Ubuntu-20.04`

Make sure you set wsl to version 2
- `wsl -l -v`
- `wsl --set-default-version [1|2]`
- `wsl -s <distro_name>` or `wsl --setdefault <distro_name>` e.g. wsl -s Debian
- search: Windows Features: Virtual Machine Platform
![vm-platform](readme-img/windows-feature-vm-platform.PNG)
- reboot

Switch to version 2
- `wsl --set-version Ubuntu 2`

Update the kernel package [more info](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

[download update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

### WSL User setup [see](https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-user-info)

`wsl -u root`
`passwd <usernam>`

### Integrating VSCode with WSL

Set up code for wsl [src](https://code.visualstudio.com/docs/remote/wsl)

- `wsl`
- `code .` if there is an error: add code to path

### Add code to path

in wsl: `nano ~/.bashrc`

- add this to end of ~/.bashrc:
`export PATH="$PATH/path/to/vscode"`

- NOTE: change the path to where your vscode installation is located
- save (ctrl-o)
- exit (ctrl-x)

Code should automatically install the wsl remote server

if successful you should see this:

![vscode-wsl-success](readme-img/wsl-code-integration-00.PNG)

---

## Install nvm / Node

Recommended:
You should get a node version manager. Node is a fragmented ecosystem. You need multiple versions to work with.



### nvm for windows if you can't use WSL
- install nvm-windows [here](https://github.com/coreybutler/nvm-windows/releases)
- IF using manual installation add:

`settings.txt`
```
root: C:\path\to\nvm\root
path: C:\path\to\nodejs
```
to `C:\`

---

### Get nvm (Node Version Manager) [here](https://github.com/nvm-sh/nvm)
- note: only works on linux platforms
- on windows, you can use WSL (Windows Subsystem for Linux)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Standalone Node (not recommended)
Install Node: https://nodejs.org/en/download/

---

### Set node version to use
You may get access denied; on elevated powershell run:
- `nvm install <version>`
- `nvm use <version>`
- `nvm current`

### Set up TypeScript
- Typescript requires nodes

## Create React project
If WSL Eperm symlink use nvm-windows

### Install Eslint
`npm install eslint` or globally `npm install --global eslint`

initialize eslint
`./node_modules/.bin/eslint --init`

Enable all `eslint-plugin-react-hooks` rules for your project.
- They are essential and help catch the most severe bugs early.

`npm install eslint-plugin-react-hooks`

### Activate format on save

Ctrl+Shift+P -> Preferences: open user settings JSON,
add
``` json
"editor.formatOnSave":true,
"[javascript]":{
  "editor.defaultFormatter": "esbenp:prettier-vscode"
}
```

---

### Install React Developer Tools
https://beta.reactjs.org/learn/react-developer-tools


---

### Install Babel to use JSX
Go into project root:
`npm init -y`
`npm install babel-cli@6 babel-preset-react-app@3`

### Create react project
- `npx create-react-app my-app`
- `npm start`

---

### Setup JSX preprocessor
preprocess JSX every time you save
1. create folder called `src`
2. in root folder of project
  - in terminal, run `npx babel --watch src --out-dir . --presets react-app/prod`
3. move JSX-ified `your-script.js` to the `src` folder


---

Deploy to production
- `npm run build`

The optimized build is in `build` folder.

Create React App only creates frontend build pipeline using Babel (javascript compiler) and webpack (bundler).

You can choose your own backend and database.


---

## How React Works

React is declarative(functional)

`render` method returns a description of what you want to see on screen

React takes the description and displays the result

Example of how React transforms input code you wrote to code that is actually run on the browser: 

e.g.
`<div />`  -> `React.createElement('div')`

Input
```js
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />

```
Output

```js
/*#__PURE__*/
React.createElement("div", {
  className: "shopping-list"
}, /*#__PURE__*/React.createElement("h1", null, "Shopping List for ", props.name), /*#__PURE__*/React.createElement("ul", null, /*#__PURE__*/React.createElement("li", null, "Instagram"), /*#__PURE__*/React.createElement("li", null, "WhatsApp"), /*#__PURE__*/React.createElement("li", null, "Oculus")));
```

`render` returns a React element

`render()` -> react-element

react-element= description of what to render

Data is passed through props which is short for properties

---
## Create tic tac toe example

Delete all files in `src` folder

Linux:  `cd src && rm -f *`

Windows: `cd src; del *`

`cd ..`

Add file named `index.css` with following code to `src/`

`index.css`
```js
body {
  font: 14px "Century Gothic", Futura, sans-serif;
  margin: 20px;
}

ol, ul {
  padding-left: 30px;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.status {
  margin-bottom: 10px;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.square:focus {
  outline: none;
}

.kbd-navigation .square:focus {
  background: #ddd;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

Add file named `index.js` with following code to `src/`

`index.js`
```js
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<Game />);
```

Add to top of `index.js` in `src/`
```js 
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
```

See Also:

npm (node package manager): handles installation of third-party javascript libraries

bundler: lets you write modular code that gets combined into a small package to optimize load time

compiler: program that transforms code from one form to another, e.g. next-gen javascript >> Babel >> browser compatible javascript