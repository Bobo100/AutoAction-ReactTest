# React 使用Github Actions

## 部屬到Github Pages

先安裝gh-pages套件

```txt
npm install gh-pages --save-dev
```

在package.json的scipr中加入下面這兩行

```json
"predeploy": "npm run build",
"deploy": "gh-pages -d build",
```

一般情形會長這樣

```json
"scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
```

在開頭加入homepage

```json
"homepage": "https://{你的githubID}.github.io/{你的專案名稱}/",
// 以我的專案例子來看
"homepage": "https://bobo100.github.io/AutoAction-ReactTest/",
```

完成後便可以執行

```txt
npm run deploy
```

## 自動部屬 GitHub Action

在你的專案下建立一個.github資料夾 之後在裡面再練立一個資料夾workflows
變可建立自動化發佈檔案，名字自己決定，附檔名要是.yml

```text
name: Deploy using Github Action

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:      
        # node-version 可以只留自己需要的版本！！
        node-version: [14.x, 16.x, 18.x]

    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm ci
    - run: npm run build --if-present
      env:
        CI: false
    # - run: npm test
    
    - name: Deploy 🚀
      uses: JamesIves/github-pages-deploy-action@v4
      with:
        folder: build # The folder the action should deploy.
```
