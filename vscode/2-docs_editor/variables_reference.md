

# Input variables

参考：[Input variables](https://code.visualstudio.com/docs/editor/variables-reference#_input-variables)

命令变量已经很强大，但它们缺乏一种机制来配置针对特定用例运行的命令。例如，无法将提示消息或默认值传递给通用的 “user input prompt”。

此限制可通过语法为 ${input：variableID} 的输入变量来解决。variableID 是指 launch.json 和 tasks.json 的 inputs 部分中的条目，其中指定了其他配置属性。不支持输入变量的嵌套。

一个tasks.json模板示例如下：

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "task name",
      "command": "${input:variableID}"
      // ...
    }
  ],
  "inputs": [
    {
      "id": "variableID",
      "type": "type of input variable"
      // type specific configuration attributes
    }
  ]
}
```



当vscode执行与launch.json、tasks.json相关的操作时，如果检测到 ${input：variableID} 就会启动一个与用户的交互。目前vscode支持如下三种类型的input variables：

- **promptString**: 显示一个输入框，用于从用户那里获取字符串。
- **pickString**: 显示 Quick Pick 下拉列表，让用户可以从多个选项中进行选择。
- **command**: 运行任意命令。