---
title: 'Windows下使用WSL(Linux子系统) + Visual Studio Code配置C/C++配置环境的教程'
date: 2019-07-01 00:21:00
tags: [C/C++,WSL,Windows, Visual Studio Code]
published: true
hideInList: false
feature: 
---
由于个人原因更换了新电脑，然后需要在新电脑上配置开发环境，于是使用了Visual Studio Code + WSL来配置简便的Linux C/C++开发环境
<!-- more -->
# 安装Visual Studio Code
这个过程非常简单，按照网上的教程一步步的进行即可。

# Visual Studio Code安装必要插件
强烈推荐Microsoft官方开发的C/C++插件。![C/C++插件](https://s2.ax1x.com/2019/07/01/Z3Yyiq.png)

# 安装WSL(Linux子系统)
在Microsoft官方商店中搜索Linux，![Microsoft商店搜索Linux](https://s2.ax1x.com/2019/07/01/Z3tk6S.png)选择喜欢的系统类型进行下载即可。笔者选择的是Ubuntu，Microsoft商店中下载的Ubuntu默认是18.04版本。下载完成后可以通过开始菜单打开，跟着提示一路完成即可。

# WSL安装必要的编译环境
WSL默认只带了gcc编译器，因此想要配置C++编译环境的同学需要自行下载g++编译器以及gdb调试工具。关于这个可以查阅Microsoft官方文档，不再过多赘述。

# Visual Studio Code配置
Visual Studio Code在settings.json中配置wsl为默认shell。![Visual Studio Code配置wsl为默认shell](https://s2.ax1x.com/2019/07/01/Z3tnkn.png)
接着配置C/C++插件，C/C++插件默认使用项目目录下的.vscode/c_cpp_properties.json文件作为配置文件。这里笔者只简单的使用了Microsoft官方提供的配置文件进行配置，文件如下：
```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "/usr/bin/g++",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64",
            "browse": {
                "path": [
                    "${workspaceFolder}"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            }
        }
    ],
    "version": 4
}
```
为了能够在Visual Studio Code中直接使用快捷键快速构建项目，你可以配置.vscode/tasks.json文件配置一些自定义的task，这里贴出官方提供的配置文件：
```json
{
     "version": "2.0.0",
     "windows": {
         "options": {
             "shell": {
                 "executable": "c:\\windows\\sysnative\\bash.exe",
                 "args": ["-c"]
             }
         }
     },
     "tasks": [
         {
             "label": "build hello world on WSL",
             "type": "shell",
             "command": "g++",
             "args": [
                 "-g",
                 "-o",
                 "/mnt/c/Users/<your user name>/Desktop/test/helloworld.out",
                 "helloworld.cpp"
             ],
             "group": {
                 "kind": "build",
                 "isDefault": true
             }
         }
     ]
 }
```
具体文件中各配置项含义，参考Microsoft提供的官方文档，这里不再赘述。
为了方便的在Visual Studio Code中调试你的代码，你可以配置.vscode/launch.json，这里同样贴出Microsoft提供的官方示例：
```json
{
"version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "/mnt/c/Users/<your user name>/Desktop/test/helloworld.out",
            "args": [""],
            "stopAtEntry": true,
            "cwd": "/mnt/c/Users/<your user name>/Desktop/test/",
            "environment": [],
            "externalConsole": true,
            "windows": {
                "MIMode": "gdb",
                "miDebuggerPath": "/usr/bin/gdb",
                "setupCommands": [
                    {
                        "description": "Enable pretty-printing for gdb",
                        "text": "-enable-pretty-printing",
                        "ignoreFailures": true
                    }
                ]
            },
            "pipeTransport": {
                "pipeCwd": "",
                "pipeProgram": "c:\\windows\\sysnative\\bash.exe",
                "pipeArgs": ["-c"],
                "debuggerPath": "/usr/bin/gdb"
            },
            "sourceFileMap": {
                "/mnt/c": "${env:systemdrive}/",
                "/usr": "C:\\Users\\...\\usr\\"
            }
        }
    ]
}
```
不明之处自行查阅官方文档。
