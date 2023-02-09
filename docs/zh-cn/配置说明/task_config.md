配置结构
```json
[
  {
    "plugins_config": [
      {
        "Plugin": {}
      }
    ]
  }
]
```
example:

```json
{
  "plugins_config": [
    {
      "Rad": {
        "target": "http://xx",
        "downstream_plugin": "Recorder"
      }
    },
    {
      "Nuclei": {
        "target": "http://xxx"
      }
    }
  ]
}
```

## plugins_config

实际下发的配置，会按照顺序执行，任务一般都是由 workflow 自动生成