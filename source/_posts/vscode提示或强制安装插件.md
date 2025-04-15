---
title: vscode提示或强制安装插件
categories: devops
tags:
  - VSCode
  - IDE
abbrlink: e7f49ea6
date: 2025-04-15 09:59:36
---


## 提示用户安装插件

### extensions.json


> 每个VSCode项目都可以有一个名为 `.vscode` 的隐藏文件夹。在这个文件夹中，你可以创建一个名为 `extensions.json` 的文件，这个文件用于**建议用户安装某些插件**。

1. 创建 `extensions.json` 文件

```bash
# 如果没有 .vscode 文件夹，创建一个
mkdir .vscode
# 在 .vscode 文件夹中创建一个名为 extensions.json 的文件：
touch .vscode/extensions.json
```

2. 编辑 `extensions.json` 文件

打开 `extensions.json` 文件，并添加以下内容：
```json
//extensions.json
{
  "recommendations": [
    "ms-python.python",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint"
  ]
}
```

在这个例子中，我建议安装`Python`、`Prettier`和`ESLint`插件。数组里就是这些插件的唯一的**标识符Extension ID**，通常是 `发布者.插件名` 的形式。


3. 保存 `extensions.json`
   
保存 `extensions.json`文件后，当其他开发者打开这个项目时，VSCode会自动检测并提示他们安装推荐的插件

## 强制安装插件
> 使用`vscode`命令行工具安装插件

1. 安装插件的命令
```sh
# install-extensions.sh
code --install-extension ms-python.python
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
```

将上述命令保存在 `install-extensions.sh` 文件中

注：为确保 `install-extensions.sh` 脚本是可执行的,需先加权限：
```sh
chmod +x install-extensions.sh
```

2. 执行脚本（可手动操作来安装插件）
```sh
bash install-extensions.sh
```



### tasks.json
> 为了在打开Vscode的时候能自动执行安装脚本，我们可以配置一个任务`tasks.json`。

1. 创建任务配置文件`tasks.json`

在你的项目目录下的 `.vscode` 文件夹中创建一个 `tasks.json` 文件（如果文件夹不存在，则创建它）。

```sh
mkdir -p .vscode
touch .vscode/tasks.json
```

2. 编辑 `tasks.json`

在 `tasks.json` 中定义一个任务来运行 `install-extensions.sh` 脚本：
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Install Extensions",
      "type": "shell",
      "command": "./install-extensions.sh",
      "problemMatcher": [],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "runOptions": {
        "runOn": "folderOpen"
      }
    }
  ]
}
```

在这里，我们定义了一个名为 “Install Extensions” 的任务，它将在打开文件夹时**自动运行**。



## 自动化插件管理
如果你想更自动化地管理插件和配置，可以考虑使用VSCode的`API`进行定制开发。通过编写一个`VSCode扩展`，在启动时自动检查和安装所需插件，甚至应用特定的配置。这需要一定的`JavaScript/TypeScript`编程技能，但可以实现高度的定制化。

以下是一个简单的示例代码，展示了如何编写一个VSCode扩展，在启动时自动安装指定的插件：
```js
const vscode = require('vscode');
function activate(context) {
    const requiredExtensions = [
        'ms-python.python',
        'esbenp.prettier-vscode',
        'dbaeumer.vscode-eslint'
    ];
    requiredExtensions.forEach(extensionId => {
        const extension = vscode.extensions.getExtension(extensionId);
        if (!extension) {
            vscode.window.showInformationMessage(`Installing ${extensionId}...`);
            vscode.commands.executeCommand('workbench.extensions.installExtension', extensionId);
        }
    });
}
exports.activate = activate;
```

## 参考链接                     
https://blog.csdn.net/m0_37890289/article/details/143632743