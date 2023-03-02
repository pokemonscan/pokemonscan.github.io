扫描一般采用 `Masscan` + `Nmap` 结合来实现

先用 `Masscan` 扫描全端口, 再利用 `Nmap` 对端口识别协议

对应`workflowTpl`如下:

```json
[
  {
    "input": {
      "plugins": [
        {
          "Masscan": {
            "target": "{{.}}",
            "ports": "60-100"
          }
        }
      ]
    }
  },
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
]
```

这个流程的输入是一个主机,先调用 Masscan 对齐 60-100 端口进行快速扫描, Masscan 只扫描到了端口开放情况, 上报的 service 是 `tcp`,
再调用 Nmap 对  Masscan 发现开放的端口扫描,补全协议内容

如果是input阶段非 masscan 插件,可以这样使用,即将 OneForAll 插件获取到的 Host 再进行一次端口扫描
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
      ]
    }
  },
  {
    "host": {
      "plugins": [
        {
          "Masscan": {
            "target": "{{.}}",
            "ports": "60-100"
          }
        }
      ]
    }
  },
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
]
```
