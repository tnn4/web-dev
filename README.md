# Setting up Web Dev Environment

## Create React project

- requires node, WSL, vscode, web browser, if you want nvm: linux

```
```
#### Install Node

Recommended:
You should get a node version manager. Node is a fragmented ecosystem. You need multiple versions to work with.

### Get nvm (Node Version Manager) [here](https://github.com/nvm-sh/nvm)
- note: only works on linux platforms
- on windows, you can use WSL (Windows Subsystem for Linux)

### nvm for windows
- install nvm-windows [here](https://github.com/coreybutler/nvm-windows/releases)
- manual installation add:
`settings.txt`

```
root: C:\path\to\nvm\root
path: C:\path\to\nodejs
```
to `C:\`

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Standalone Node (not recommended)
Install Node: https://nodejs.org/en/download/

---
### Using node with windows 
### Set up WSL and VSCode

- install WSL [src](https://docs.microsoft.com/en-us/windows/wsl/install)
- set up code for wsl [src](https://code.visualstudio.com/docs/remote/wsl)

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

### Install Eslint
`npm install eslint` or globally `npm install --global eslint`

initialize eslint
`./node_modules/.bin/eslint --init`

Enable all `eslint-plugin-react-hooks` rules for your project.
- They are essential and help catch the most severe bugs early.

`npm install eslint-plugin-react-hooks`

Activate format on save

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