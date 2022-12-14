# vscode

## Extensions

VS Code功能扩展依赖于大量插件。

点击左侧工具栏中的Extensions，会弹出插件列表。注意，插件分为本地Local和远端，本地代表的是当前安装VSCode的计算机，而远端一般是通过ssh远程连接的计算机，例如局域网中的另外一个电脑或者WSL2。

如果是在远端计算机上开发，一定要确保远端计算机上安装了你需要的插件，例如我当前连接WSL，插件列表如下所示：

![image-20220908151915049](imgs\image-20220908151915049.png)

你可以通过点击远端WSL下载图标，下载对应的插件。

## settings

打开设置

- ctrl+,
- 文件->首选项->设置

### C++智能提示

选择设置->Command Palette（快捷键ctrl+shift+P)，输入configuration，选择c/c++ Edit configuration，自动在当前目录下生成并打开c_cpp_properties.json：

![image-20220908152710497](imgs\image-20220908152710497.png)

在`includePath`中添加头文件路径即可！

例如我进行ROS2开发时，添加如下头文件：

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "/opt/ros/foxy/include",
                "/usr/include",
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "compilerPath": "/opt/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-gcc",
            "cStandard": "gnu17",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "linux-gcc-arm"
        }
    ],
    "version": 4
}
```

这种方式，对于远程连接开发也是有效的。

生成的

## 插件推荐

### Remote Development

- Remote SSH
- Remote WSL
- Remote Containter

### C++

Todo tree

Doxygen Document

## 插件使用

### koroFileheader

1. **配置**

打开设置，搜索fileheader，打开settings.json，添加如下：

```json
    "fileheader.customMade": { //此为头部注释
        "Description": "", // 文件描述
        "Version": "2.0", // 版本
        "Author": "xueyuankui", // 文件作者
        "Date": "Do not edit", // 文件创建时间(不变)
        "LastEditors": "xueyuankui", // 文件最后编辑者
        "LastEditTime": "Do not edit" // 文件最后编辑时间
    },
    "fileheader.cursorMode": { //此为函数注释
        "description":"", // 功能描述
        "param": "", // 传入参数
        "return": "", // 返回结果
        "author":"xueyuankui" // 函数作者
    }
```

2. **使用**

Ctrl + Win+i 文件头部注释快捷键

Ctrl + Win+ t 函数注释

