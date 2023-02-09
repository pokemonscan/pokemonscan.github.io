## 工作流程

整个 workflow 会以顺序的方式生成 `task`    
如在 `input` 阶段，其中的变量是 `{{.}}`，会自动将 workflow 中的输入替换。   
在其余阶段，会根据 `workflow_space` 的选项，从数据库中提取相应的数据，格式化到配置文件中，生成新的扫描任务

其中 `workflow_space` 有两种选项
- workflow （默认值）
  > 只对当前 `workflow` 产生的资产类型扫描
- workspace
  > 对当前整个 `workspace` 的资产类型扫描

配置结构
```json
[
  {
    "工作阶段": {
      "plugins": [
      ],
      "workflow_space": "workflow"
    }
  }
]
```
example:
```json
[
  {
    "input": {
      "plugins": [
        {
          "OneForAll": {
            "command_args": "--alive true --target {{.}}",
            "timeout": 1000
          }
        },
        {
          "Fofa": {
            "type": "query",
            "query": "cert=\"{{.}}\"",
            "timeout": 600
          }
        },
        {
          "Fofa": {
            "type": "query",
            "query": "domain=\"{{.}}\"",
            "timeout": 600
          }
        }
      ],
      "workflow_space": "workflow"
    }
  },
  {
    "website": {
      "plugins": [
        {
          "Rad": {
            "target": "{{.Url}}",
            "downstream_plugin": "Recorder"
          }
        },
        {
          "Nuclei": {
            "target": "{{.Url}}"
          }
        }
      ],
      "workflow_space": "workflow"
    }
  }
]
```

## 工作阶段

### input

变量：`{{.}}`

### website

变量: `{{.Url}}`