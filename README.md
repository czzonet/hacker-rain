HackerRain--黑客帝国数字雨
=

偶然见到他人项目还原数字雨，就学习一下别人的代码。正好最近看旧版的JavaScript高级程序设计，看到项目里也有一些旧版的js使用方法。可见由于js变动太快，所以有极大的跨度，旧的arguments用法和新的class混杂，更不用说现在的typescript了。

这里先用node+ts环境测试函数的使用，弄清数字雨的原理。

从头构建
--

难以置信，竟然需要这么多步骤，如何建立自己的脚手架？目前暂时用手动梳理步骤，以后使用就拷贝即可。

1. README.md
   ```
   touch README.md
   ```
2. git
   ```
   git init
   touch .gitignore
   ```
3. tsc
   ```sh
   mkdir src
   touch tsconfig.json
   ```
4. yarn
   ```
   yarn init
   ```

.gitignore文件内容
--
排除生成的dist，npm模块，和yarn的锁定文件。注意不需要的文件要及时排除，一旦提交就会永久存在文件记录里，特别对于大型文件，极难排除。

``` 
dist
node_modules
yarn.lock
```

tsconfig.json 文件内容
--

一些编译选项，没有太多特别。不要过多，自己定义，自己用的舒适。

```json
{
  "compilerOptions": {
    "target": "es2015",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "outDir": "dist",
    "strict": true,
    "alwaysStrict": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "noImplicitReturns": true,
    "lib": [
      "es2015",
      "es2016"
    ],
    "typeRoots": [
      "./node_modules/@types"
    ]
  },
  "include": [
    "./src/**/*"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

vs code 调试
--

先`Terminal->Configure Tasks`添加ts的build任务，再`Debug->Add Configuration`添加node的调试配置，基本是全部默认生成的，只有少部分需要手动指定。

tasks.json 文件内容
--

由于windows下解析路径`\\`的问题，所以要指定`power shell`来运行，最后如下：

```json
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558 
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      "type": "typescript",
      "tsconfig": "tsconfig.json",
      "problemMatcher": [
        "$tsc"
      ],
      "options": {
        "shell": {
          "executable": "powershell.exe"
        }
      }
    }
  ]
}
```

launch.json 内容
--

手动指定可执行文件位置就可以了，最后如下：

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}\\dist\\index.js",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": [
        "${workspaceFolder}/dist/**/*.js"
      ]
    }
  ]
}
```

startup分支
--

基础构建完毕，可以调试运行了，推送到远程startup分支，以后起步就拉取该分支即可。

