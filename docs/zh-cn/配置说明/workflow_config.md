## 工作流程

整个 workflow 会以在使用 go-temple 解析配置后,顺序生成 `task`    
如在 `input` 阶段，其中的变量是 `{{.}}`，会自动将 workflow 中的输入替换。   
在其余阶段，会根据 `workflow_space` 的选项，从数据库中提取相应的数据，格式化到配置文件中，生成新的扫描任务



配置结构
```json
[
  {
    "工作阶段": {
      "plugins": [
      ],
      "workflow_space": "workflow"
    },
    "workflow_space": "workflow",
    "filter": " tag = 'OSS' "
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
      "workflow_space": "workflow",
      "filter": ""
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
      "workflow_space": "workflow",
      "filter": ""
    }
  }
]
```

### 参数配置

### workflow_space
其中 `workflow_space` 有两种选项
- workflow （默认值）
  > 只对当前 `workflow` 产生的资产类型扫描
- workspace
  > 对当前整个 `workspace` 的资产类型扫描

### filter
`filter` 字段的内容会作为 where 条件拼接到查询语句后.   
比如我们现在的需求是这样的:

> 将当前 workflow 中扫描出的所有 tag 为 `OSS` 的 Url 取出 , 作为 `OSS_Hack` 插件的输入,那么此阶段的配置应该是

```json
{
  "url": {
    "plugins": [
      {
        "OSS_HACK": {
          "target": "{{.Url}}"
        }
      }
    ],
    "workflow_space": "workflow",
    "filter": " tag = 'OSS' "
  }
}
```


## 工作阶段

### input

变量：`{{.}}`

### website

变量: `{{.Url}}`


### host

变量: `{{.Host}}`
方法: `GetPorts`
demo:
```json
{
  "host": {
    "plugins": [
      {
        "Nmap": {
          "target": "{{.Host}}",
          "ports": "{{ .GetPorts \"service='tcp'\"}}"
        }
      }
    ],
    "workflow_space": "workspace"
  }
}
```

### url

变量: `{{.Url}}`
