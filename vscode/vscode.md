# vscode

## settings

打开设置

- ctrl+,
- 文件->首选项->设置

## 插件

#### Remote Development

- Remote SSH
- Remote WSL
- Remote Containter

#### koroFileheader

1. **配置**

打开设置，搜索fileheader，打开settings.json，添加如下：

```json
    "fileheader.customMade": { //此为头部注释
        "Description": "", // 文件描述
        "Version": "2.0", // 版本
        "Author": "wuyue.nan", // 文件作者
        "Date": "Do not edit", // 文件创建时间(不变)
        "LastEditors": "wuyue.nan", // 文件最后编辑者
        "LastEditTime": "Do not edit" // 文件最后编辑时间
    },
    "fileheader.cursorMode": { //此为函数注释
        "description":"", // 功能描述
        "param": "", // 传入参数
        "return": "", // 返回结果
        "author":"wuyue.nan" // 函数作者
    }
```

2. **使用**

Ctrl + Win+i 文件头部注释快捷键

Ctrl + Win+ t 函数注释

### C++



Todo tree

Doxygen Document