---
title: Error Cannot find module 'webpack-cli/bin/config-yargs'
categories: [Error]
tags: [Node.js, webpack, webpack-dev-server]
---

Error: Cannot find module 'webpack-cli/bin/config-yargs'를 해결해보자. 

webpack dev server를 실행할 때 config-yargs 모듈을 찾을 수 없었다.  이는 webpack과 webpack dev server간의 충돌로 발생하는 에러로 이 둘을 버전을 맞춰주면 해결할 수 있다.

```npm install --save-dev webpack@4.35.3  webpack-cli@3.3.5  webpack-dev-server@3.7.2```



