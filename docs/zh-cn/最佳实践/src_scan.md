一个通用的 src 扫描流程大致如下

资产范围收集 -> 子域名扫描 -> 漏洞扫描 -> 手动复测

work_config 中我们注册 `PassiveXray` 和 `Recorder` 两个插件

```json
{
  "plugins": [
    {
      "PassiveXray": {
        "listen_port": 7777

      }
    },
    {
      "Recorder": {
        "whitelist_type": [
          "jpg",
          "gif",
          "png",
          "css"
        ],
        "request_intercept_rules": {
          "uri": {
            "login_page": "login",
            "register_page": "register"
          },
          "headers": {
          },
          "parameters": {
            "RCE Paramters": "((cmd=)|(exec=)|(command=)|(execute=)|(ping=)|(query=)|(jump=)|(code=)|(reg=)|(do=)|(func=)|(arg=)|(option=)|(load=)|(process=)|(step=)|(read=)|(function=)|(feature=)|(exe=)|(module=)|(payload=)|(run=)|(daemon=)|(upload=)|(dir=)|(download=)|(log=)|(ip=)|(cli=))",
            "Debug Logic Parameters": "((access=)|(adm=)|(admin=)|(alter=)|(cfg=)|(clone=)|(config=)|(create=)|(dbg=)|(debug=)|(delete=)|(disable=)|(edit=)|(enable=)|(exec=)|(execute=)|(grant=)|(load=)|(make=)|(modify=)|(rename=)|(reset=)|(root=)|(shell=)|(test=)|(toggl=))",
            "URL As A Value": "(=(https?://.*|https?%3(a|A)%2(f|F)%2(f|F).*))",
            "Upload Form": "(type=\\\"file\\\")",
            "DoS Paramters": "((size=)|(page=)|(num=))",
            "OSS": "([A|a]ccess[K|k]ey[I|i][d|D]|[A|a]ccess[K|k]ey[S|s]ecret)"
          }
        },
        "response_intercept_rules": {
          "headers": {
            "Shiro": "(=deleteMe|rememberMe=)"
          },
          "body": {
            "Swagger UI": "((swagger-ui.html)|(\\\"swagger\\\":)|(Swagger UI)|(swaggerUi))",
            "Ueditor": "(ueditor\\.(config|all)\\.js)",
            "Chinese IDCard": "[^0-9]((\\d{8}(0\\d|10|11|12)([0-2]\\d|30|31)\\d{3}$)|(\\d{6}(18|19|20)\\d{2}(0[1-9]|10|11|12)([0-2]\\d|30|31)\\d{3}(\\d|X|x)))[^0-9]",
            "Chinese Mobile Number": "[^\\w]((?:(?:\\+|00)86)?1(?:(?:3[\\d])|(?:4[5-79])|(?:5[0-35-9])|(?:6[5-7])|(?:7[0-8])|(?:8[\\d])|(?:9[189]))\\d{8})[^\\w]",
            "Internal IP Address": "[^0-9]((127\\.0\\.0\\.1)|(10\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3})|(172\\.((1[6-9])|(2\\d)|(3[01]))\\.\\d{1,3}\\.\\d{1,3})|(192\\.168\\.\\d{1,3}\\.\\d{1,3}))",
            "MAC Address": "(^([a-fA-F0-9]{2}(:[a-fA-F0-9]{2}){5})|[^a-zA-Z0-9]([a-fA-F0-9]{2}(:[a-fA-F0-9]{2}){5}))",
            "Chinese Bank Card ID": "[^0-9]([1-9]\\d{12,18})[^0-9]",
            "Java Deserialization": "(javax\\.faces\\.ViewState)",
            "JSONP Response": "(^(\\w.*?)\\()",
            "Amazon AWS URL": "(((([a-zA-Z0-9._-]+\\.s3|s3)(\\.|\\-)+[a-zA-Z0-9._-]+|[a-zA-Z0-9._-]+\\.s3|s3)\\.amazonaws\\.com)|(s3:\\/\\/[a-zA-Z0-9-\\.\\_]+)|(s3.console.aws.amazon.com\\/s3\\/buckets\\/[a-zA-Z0-9-\\.\\_]+)|(amzn\\.mws\\.[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12})|(ec2-[0-9-]+.cd-[a-z0-9-]+.compute.amazonaws.com)|(us[_-]?east[_-]?1[_-]?elb[_-]?amazonaws[_-]?com))",
            "Amazon AWS AccessKey ID": "[^0-9]((aws(.{0,20})?(?-i)[''\\\"][0-9a-zA-Z\\/+]{40}[''\\\"])|((A3T[A-Z0-9]|AKIA|AGPA|AIDA|AROA|AIPA|ANPA|ANVA|ASIA)[a-zA-Z0-9]{16}))[^0-9]",
            "Amazon AWS Region": "((us(-gov)?|ap|ca|cn|eu|sa)-(central|(north|south)?(east|west)?)-\\d)",
            "SSH Private Key": "([-]+BEGIN [^\\s]+ PRIVATE KEY[-])",
            "Windows File/Dir Path": "[^\\w](([a-zA-Z]:\\\\(?:\\w+\\\\?)*)|([a-zA-Z]:\\\\(?:\\w+\\\\)*\\w+\\.\\w+))",
            "Password Field": "((|'|\")([p](ass|wd|asswd|assword))(|'|\")(:|=)( |)('|\")(.*?)('|\")(|,))",
            "Username Field": "((|'|\")([u](ser|name|ame|sername))(|'|\")(:|=)( |)('|\")(.*?)('|\")(|,))",
            "WeCom Key": "([c|C]or[p|P]id|[c|C]orp[s|S]ecret)",
            "JDBC Connection": "(jdbc:[a-z:]+://[a-z0-9\\.\\-_:;=/@?,&]+)",
            "Authorization Header": "((basic [a-z0-9=:_\\+\\/-]{5,100})|(bearer [a-z0-9_.=:_\\+\\/-]{5,100}))",
            "Github Access Token": "([a-z0-9_-]*:[a-z0-9_\\-]+@github\\.com*)",
            "Microsoft Teams Webhook": "(https://outlook\\.office\\.com/webhook/[a-z0-9@-]+/IncomingWebhook/[a-z0-9-]+/[a-z0-9-]+)"
          }
        },
        "max_response_length": 2048,
        "listen_port": 8980,
        "downstream_plugin": "PassiveXray"
      }

    }
  ],
  "mem_limit": 90,
  "cpu_limit": 60,
  "heart_beat_time": 30,
  "debug_mode": false
}
```


> Recorder 配置改自 `https://github.com/gh0stkey/HaE.git` 中的默认配置

workflow 模板中我们配置以下几个插件

```json
[
  {
    "input": {
      "plugins": [
        {
          "ENScan": {
            "target": "{{.}}",
            "type": "aqc"
          }
        }
      ]
    }
  },
  {
    "domain": {
      "plugins": [
        {
          "OneForAll": {
            "command_args": "--alive true --target {{.Name}}",
            "timeout": 1000
          }
        },
        {
          "Fofa": {
            "email": "",
            "key": "",
            "type": "query",
            "query": "cert=\"{{.Name}}\"",
            "timeout": 600
          }
        },
        {
          "Fofa": {
            "email": "",
            "key": "",
            "type": "query",
            "query": "domain=\"{{.Name}}\"",
            "timeout": 600
          }
        }
      ],
      "workflow_space": "workflow",
      "filter": "root = true"
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

然后 wrokflow 内输入你所测试的公司名称    
点击开始后会自动执行以下操作

- 调用 `ENScan` 查找相关公司以及根域名，获取到 `Domain` 资产 , `ExtraInfo` 资产
- 将刚刚的根域名带入`OneforAll`插件，创建任务，扫描相关网站，获取到 `Website` `Domain` `Host`资产
- 对本次发现的 `Website` ，创建任务，利用 `Rad` -> `Recorder` -> `PassiveXray`  和 `Nuclei` 对资产进行扫描， 
等待`Vul`以及`Url`上报


喝杯咖啡☕️，然后去复测漏洞是否有效，以及观察`Url`资产中是否有可以利用的点

