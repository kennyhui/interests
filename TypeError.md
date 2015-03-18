#### 问题描述

今天在使用 webpack + react 开发 [https://github.com/yibuyisheng/bimawen](https://github.com/yibuyisheng/bimawen) 的时候，突然遇到了一个报错：
 
 ```
 TypeError: Cannot read property 'firstChild' of undefined
 ```
 
这个错误报在了 react 源码中，当时我的 package.json 配置如下：
 
 ```json
 {
  "name": "bimawen",
  "version": "1.0.0",
  "description": "",
  "main": "gulpfile.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.8.11",
    "gulp-concat": "^2.5.2",
    "gulp-jsx": "git://github.com/yibuyisheng/gulp-jsx",
    "gulp-less": "^3.0.1",
    "jsx-loader": "^0.12.2",
    "q": "^1.2.0",
    "webpack": "^1.7.3"
  },
  "dependencies": {
    "react": "^0.12.2",
    "react-router": "^0.12.4"
  }
}
 ```
 
 从中可以看出 react 和 react-router 的版本。

#### 问题解决

 实在想不出是什么问题，于是 google，找到这个地方：[http://stackoverflow.com/questions/27153166/typeerror-when-using-react-cannot-read-property-firstchild-of-undefined](http://stackoverflow.com/questions/27153166/typeerror-when-using-react-cannot-read-property-firstchild-of-undefined)，总结一下回答中所说的能造成这个报错的因素：
 
 * 1、引入了两个不同版本的 react。例如，在 package.json 的 dependencies 中引入了不同版本的 react ，而不是在 peerDependencies 中引入的，这就有可能在类似于 node_modules/<some library using React>/node_modules/react 的目录下安装一个独立的 react 。到目前为止，两个版本的 react 混用是不能正常工作的。
 * 2、在不同的文件中分别使用了 require('react') 和 require('react/addons')。
 * 3、在不同的文件中分别使用了 require('react') 和 require('React')。
 
经过排查，此处的错误是第三条造成的。
 
