配置结构

```json
{
  "plugins": [
    {
      "PassiveXray": {
        "listen_port": 7777

      }
    },
    {
      "Fofa": {
        "email": "",
        "key": ""
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

## plugins

由于部分插件处于 `daemon` 工作状态，所以需要在注册阶段注册一个插件，下发相关配置。

> 当前版本需要注册的插件 
>  - `PassiveXray`  
>  - `Recorder`

这里需要注意一下注册顺序，如 `Recorder` 需要设置代理为 `PassiveXray`插件，
那么`PassiveXray`插件一定要在`Recorder`前注册 ，这样才能获取到`PassiveXray`的监听地址

## mem_limit

worker 内存占用限制

## cpu_limit

cpu 占用限制

## heart_beat_time

心跳时间，可以理解为 worker 会在每次心跳的时候拉取需要执行的任务。
如果心跳过快，可能会导致 worker 拉取任务过快。
建议在性能较差的机器上调高这个值。防止一个任务在初始扫描阶段性能占用不高，但后续扫描阶段变高，此时已经拉取了过多的任务。
导致 worker 过载卡死

## debug_mode

在 debug 工作模式下，会开启 pprof ，可以拉取火焰图分析性能阻塞点。