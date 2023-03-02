插件从运行状态上分为两种
- 一种是在 worker 初始注册时候下发一次配置，如被动扫描器（由于不知道扫描结束时间，所以一个worker只运行一个容器），
  Record （监听一个端口）
- 一种是每次任务下发一次配置，如主动式扫描器，信息收集工具


## 运行时配置


### Fofa

> fofa 网络空间测绘引擎  
> https://fofa.info/

 

#### 完整配置
```json
{
  "email": "",
  "key": "",
  "query": "",
  "alive": false,
  "type": "query",
  "timeout": 100
}
```

#### 最简配置
```json
{
  "email": "",
  "key": "",
  "query": "",
  "type": "query",
}
```
#### 配置说明

- email ： fofa email 配置，无此配置插件不生效
- key : fofa key 配置，无此配置插件不生效
- query: fofa 查询语法
- type: fofa api 中的type ，目前只支持 `query` 类型
- alive: 是否进行站点联通性检验 （10s 超时时间）, 默认为 false
- timeout: 当进行站点连通性检验时候，整体任务的超时时间，当超时时会防止卡死直接结束（一般不会出现这种情况）

#### 返回结果

- website
- host
- domain


### ENScan

> 国内企业信息收集工具   
> 仓库地址： https://github.com/wgpsec/ENScan_GO

#### 完整配置

```json
{
  "target": "小米",
  "type": ""
}
```
#### 最简配置

略

#### 配置说明

- target: 原始 ENScan 中 -n 参数
- type: 原始 ENScan 中 -type 参数，目前只有三种选项
  - tyc
  - aqc
  - aqc,tyc

#### 返回结果

- extra_info
- domain


### OneForAll

> 一款强大的资产收集工具    
> 仓库地址： https://github.com/shmilylty/OneForAll.git

#### 完整配置

```json
{
  "api_py": "IyDmqKHlnZdBUEnphY3nva4KIyBDZW5zeXPlj6/ku6XlhY3otLnms6jlhozojrflj5ZBUEnvvJpodHRwczovL2NlbnN5cy5pby9hcGkKY2Vuc3lzX2FwaV9pZCA9ICcnCmNlbnN5c19hcGlfc2VjcmV0ID0gJycKCiMgQmluYXJ5ZWRnZeWPr+S7peWFjei0ueazqOWGjOiOt+WPlkFQSe+8mmh0dHBzOi8vYXBwLmJpbmFyeWVkZ2UuaW8vYWNjb3VudC9hcGkKIyDlhY3otLnnmoRBUEnmnInmlYjmnJ/lj6rmnIkx5Liq5pyI77yM5Yiw5pyf5LmL5ZCO5Y+v5Lul5YaN5qyh55Sf5oiQ77yM5q+P5pyI5Y+v5Lul5p+l6K+iMjUw5qyh44CCCmJpbmFyeWVkZ2VfYXBpID0gJycKCiMgQmVWaWdpbCBBUEk6IGh0dHBzOi8vYmV2aWdpbC5jb20vb3NpbnQtYXBpCmJldmlnaWxfYXBpID0gJycKCiMgQ2hpbmF65Y+v5Lul5YWN6LS55rOo5YaM6I635Y+WQVBJ77yaaHR0cDovL2FwaS5jaGluYXouY29tL0FwaURldGFpbHMvQWxleGEKY2hpbmF6X2FwaSA9ICcnCgojIEJpbmflj6/ku6XlhY3otLnms6jlhozojrflj5ZBUEnvvJpodHRwczovL2F6dXJlLm1pY3Jvc29mdC5jb20vemgtY24vc2VydmljZXMvCiMgY29nbml0aXZlLXNlcnZpY2VzL2Jpbmctd2ViLXNlYXJjaC1hcGkvI3dlYi1qc29uCmJpbmdfYXBpX2lkID0gJycKYmluZ19hcGlfa2V5ID0gJycKCiMgU2VjdXJpdHlUcmFpbHPlj6/ku6XlhY3otLnms6jlhozojrflj5ZBUEnvvJpodHRwczovL3NlY3VyaXR5dHJhaWxzLmNvbS9jb3JwL2FwaQpzZWN1cml0eXRyYWlsc19hcGkgPSAnJwoKIyBodHRwczovL2ZvZmEuc28vYXBpCmZvZmFfYXBpX2VtYWlsID0gJycgICMgZm9mYeeUqOaIt+mCrueusQpmb2ZhX2FwaV9rZXkgPSAnJyAgIyBmb2Zh55So5oi3a2V5CgojIEdvb2dsZeWPr+S7peWFjei0ueazqOWGjOiOt+WPlkFQSToKIyDlhY3otLnnmoRBUEnlj6rog73mn6Xor6LliY0xMDDmnaHnu5PmnpwKIyBodHRwczovL2RldmVsb3BlcnMuZ29vZ2xlLmNvbS9jdXN0b20tc2VhcmNoL3YxL292ZXJ2aWV3I3NlYXJjaF9lbmdpbmVfaWQKIyDliJvlu7roh6rlrprkuYnmkJzntKLlvJXmk47lkI7pnIDopoHlnKjlk43lupTnmoTmjqfliLbpnaLmnb/kuIrlkK/nlKhTZWFyY2ggdGhlIGVudGlyZSB3ZWIKZ29vZ2xlX2FwaV9pZCA9ICcnICAjIEdvb2dsZSBBUEnoh6rlrprkuYnmkJzntKLlvJXmk45pZAojIGh0dHBzOi8vZGV2ZWxvcGVycy5nb29nbGUuY29tL2N1c3RvbS1zZWFyY2gvdjEvb3ZlcnZpZXcjYXBpX2tleQpnb29nbGVfYXBpX2tleSA9ICcnICAjIEdvb2dsZSBBUEnoh6rlrprkuYnmkJzntKJrZXkKCiMgaHR0cHM6Ly9hcGkucGFzc2l2ZXRvdGFsLm9yZy9hcGkvZG9jcy8Kcmlza2lxX2FwaV91c2VybmFtZSA9ICcnCnJpc2tpcV9hcGlfa2V5ID0gJycKCiMgU2hvZGFu5Y+v5Lul5YWN6LS55rOo5YaM6I635Y+WQVBJOiBodHRwczovL2FjY291bnQuc2hvZGFuLmlvL3JlZ2lzdGVyCiMg5YWN6LS555qEQVBJ6ZmQ6YCfMeenkuafpeivojHmrKEKc2hvZGFuX2FwaV9rZXkgPSAnJwojIFRocmVhdEJvb2sgQVBJIOafpeivouWtkOWfn+WQjemcgOimgeaUtui0uSBodHRwczovL3gudGhyZWF0Ym9vay5jbi9ub2RldjQvdmI0L215QVBJCnRocmVhdGJvb2tfYXBpX2tleSA9ICcnCgojIFZpcnVzVG90YWzlj6/ku6XlhY3otLnms6jlhozojrflj5ZBUEk6IGh0dHBzOi8vZGV2ZWxvcGVycy52aXJ1c3RvdGFsLmNvbS9yZWZlcmVuY2UKdmlydXN0b3RhbF9hcGlfa2V5ID0gJycKCiMgaHR0cHM6Ly93d3cuem9vbWV5ZS5vcmcvZG9jP2NoYW5uZWw9YXBpCnpvb21leWVfYXBpX2tleSA9ICcnCgojIFNweXNl5Y+v5Lul5YWN6LS55rOo5YaM6I635Y+WQVBJOiBodHRwczovL3NweXNlLmNvbS8Kc3B5c2VfYXBpX3Rva2VuID0gJycKCiMgaHR0cHM6Ly93d3cuY2lyY2wubHUvc2VydmljZXMvcGFzc2l2ZS1kbnMvCmNpcmNsX2FwaV91c2VybmFtZSA9ICcnCmNpcmNsX2FwaV9wYXNzd29yZCA9ICcnCgojIGh0dHBzOi8vd3d3LmRuc2RiLmluZm8vCmRuc2RiX2FwaV9rZXkgPSAnJwoKIyBpcHY0aW5mb+WPr+S7peWFjei0ueazqOWGjOiOt+WPlkFQSTogaHR0cDovL2lwdjRpbmZvLmNvbS90b29scy9hcGkvCiMg5YWN6LS555qEQVBJ5pyJ5pWI5pyf5Y+q5pyJMuWkqe+8jOWIsOacn+S5i+WQjuWPr+S7peWGjeasoeeUn+aIkO+8jOavj+WkqeWPr+S7peafpeivojUw5qyh44CCCmlwdjRpbmZvX2FwaV9rZXkgPSAnJwoKIyBodHRwczovL2dpdGh1Yi5jb20vMzYwbmV0bGFiL2ZsaW50CiMgcGFzc2l2ZWRuc19hcGlfYWRkcum7mOiupOepuuS9v+eUqGh0dHA6Ly9hcGkucGFzc2l2ZWRucy5jbgojIHBhc3NpdmVkbnNfYXBpX3Rva2Vu5Y+v5Li656m6CnBhc3NpdmVkbnNfYXBpX2FkZHIgPSAnJwpwYXNzaXZlZG5zX2FwaV90b2tlbiA9ICcnCgojIEdpdGh1YiBUb2tlbuWPr+S7peiuv+mXrmh0dHBzOi8vZ2l0aHViLmNvbS9zZXR0aW5ncy90b2tlbnPnlJ/miJAsdXNlcuS4ukdpdGh1YueUqOaIt+WQjQojIOeUqOS6juWtkOWfn+aOpeeuoeWSjOWtkOWfn+aUtumbhgpnaXRodWJfYXBpX3VzZXIgPSAnJwpnaXRodWJfYXBpX3Rva2VuID0gJycKCiMgb2J0YWluIENsb3VkZmxhcmUgQVBJIGtleSBmcm9tIGh0dHBzOi8vZGFzaC5jbG91ZGZsYXJlLmNvbS9wcm9maWxlL2FwaS10b2tlbnMKY2xvdWRmbGFyZV9hcGlfdG9rZW4gPSAnJwoKIyBodHRwczovL2h1bnRlci5xaWFueGluLmNvbS9ob21lL3VzZXJJbmZvCmh1bnRlcl9hcGlfa2V5ID0gJycKCiMgaHR0cHM6Ly9hcGktZG9jcy5mdWxsaHVudC5pby8KZnVsbGh1bnRfYXBpX2tleSA9ICcnCgoKIyDnmbvlvZVxdWFrZeS5i+WQjuWPr+WcqOS4quS6uuS4reW/g+iOt+WPlmtleSBodHRwczovL3F1YWtlLjM2MC5uZXQvcXVha2UvIy9wZXJzb25hbD90YWI9bWVzc2FnZQpxdWFrZV9hcGlfa2V5ID0gJycKCiNodHRwczovL3d3dy5yYWNlbnQuY29tL2N0bG9nIEYyPk5ldHdvcmvmipPljIXojrflj5ZUb2tlbgpyYWNlbnRfYXBpX3Rva2VuID0gJycK",
  "timeout": 300,
  "command_args": "--alive true --target xxx.com",
  "alive": false
}
```
#### 最简配置

```json
{
  "command_args": "--alive true --target xxx.com"
}
```


#### 配置说明

- api_py : oneforlall 中的配置文件 `api.py` base64字符 ，不填的话会用默认的`api.py`，强烈建议配置
- alive: 是否进行站点联通性检验 （10s 超时时间）, 默认为 false
- timeout: 当进行站点连通性检验时候，整体任务的超时时间，当超时时会防止卡死直接结束（一般不会出现这种情况）
- command_args: oneforall 中的命令行参数，建议 `--alive true --target xxx.com`

#### 返回结果

- website
- host
- domain

### Masscan

> 端口扫描工具
> 仓库地址： https://github.com/robertdavidgraham/masscan.git

#### 完整配置

```json
{
  "Masscan": {
    "target": "{{.}}",
    "ports": "60-100",
    "command_args": ""
  }
}
```


#### 最简配置

```json
{
  "Masscan": {
    "target": "{{.}}",
    "ports": "60-100"
  }
}
```

#### 配置说明

- target: 待扫描主机的`host`

#### 返回结果

- `Host` 
- `HostService`

### Nmap

> 端口扫描工具
> 仓库地址： https://github.com/nmap/nmap.git
> 建议和 `Masscan` 联合使用,参考 [端口扫描实践](/zh-cn/最佳实践/port_scan)

#### 完整配置

```json
{
  "Masscan": {
    "target": "{{.}}",
    "ports": "60-100",
    "command_args": ""
  }
}
```


#### 最简配置

```json
{
  "Masscan": {
    "target": "{{.}}",
    "ports": "60-100"
  }
}
```

#### 配置说明

- target: 待扫描主机的`host`

#### 返回结果

- `Host`
- `HostService`


### Nuclei

> 主动扫描器      
> 仓库地址： https://github.com/projectdiscovery/nuclei.git

#### 完整配置

```json
{
  "target": "https://xxx.com"
}
```
#### 最简配置

略

#### 配置说明

- target: 待扫描网站的`url`

#### 返回结果

- vul

### Rad

> 主动爬虫   
> 仓库地址： https://github.com/chaitin/rad.git

#### 完整配置

```json
{
  "target": "https://xxx.com",
  "downstream_plugin": "",
  "rad_config_file": "",
  "cookie": ""
}
```

#### 最简配置

```json
{
  "target": "https://xxx.com"
}
```

#### 配置说明

- target: 待扫描网站的`url`
- downstream_plugin: 上游监听插件，相当于原始的 `--listen` 参数后拼上想要传给的插件监听地址，这里直接填写插件名即可，如`Recorder`
- cookie: 爬虫所带的cookie
- rad_config_file: 爬虫配置文件的base64,**当存在此配置时候，cookie 配置不生效** 

#### 返回结果

None




## 注册时配置

### PassiveXray

> 被动式扫描引擎    
> https://github.com/chaitin/xray

### 完整配置

```json
{
  "config_file": "",
  "xray_config_file": "",
  "plugin_xray_file": "",
  "module_xray_file": "",
  "cert_file": "",
  "ca_file": "",
  "listen_port": ""
}
```

#### 最简配置

```json
{
  "listen_port": ""
}
```

#### 配置说明

- config_file: xray config.yaml base64 字符串
- xray_config_file: xray xray.yamll base64 字符串
- plugin_xray_file: xray plugin.xray.yaml  base64 字符串
- module_xray_file: xray module.xray.yaml base64 字符串
- listen_port: 被动监听地址

#### 返回结果

- vul


### Recorder

> PokemonScan 内置的包过滤器，你可以在这里配置你的规则
> 你可以在主动爬虫后接入`Recorder`插件，他会标记下你所关注的信息.
> 你可以对记录到的url进行手动测试，也可以在后续的workflow中 通过 url 阶段的 fitter: "tag = xxx" 传给后续的插件

#### 完整配置

```json
 {
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
      "RCE Paramters": "((cmd=)|(exec=)|(command=)|(execute=)|(ping=)|(query=)|(jump=)|(code=)|(reg=)|(do=)|(func=)|(arg=)|(option=)|(load=)|(process=)|(step=)|(read=)|(function=)|(feature=)|(exe=)|(module=)|(payload=)|(run=)|(daemon=)|(upload=)|(dir=)|(download=)|(log=)|(ip=)|(cli=))"
    }
  },
  "response_intercept_rules": {
    "headers": {
      "Shiro": "(=deleteMe|rememberMe=)"
    },
    "body": {
      "Microsoft Teams Webhook": "(https://outlook\\.office\\.com/webhook/[a-z0-9@-]+/IncomingWebhook/[a-z0-9-]+/[a-z0-9-]+)"
    }
  },
  "max_response_length": 20480
}
```

#### 最简配置

```json
 {
  "max_response_length": 20480
}
```

#### 配置说明

- whitelist_type: 当 urlpath 以此结尾的时候会不进行正则提取
- request_intercept_rules: 请求包中进行正则提取
  - uri ：对url进行正则提取， key 为规则名称，value 为正则
  - headers: 对header进行正则提取， key 为规则名称，value 为正则
  - parameters: 对GET/POST参数进行正则提取， key 为规则名称，value 为正则
- response_intercept_rules： 返回包中进行正则提取
  - headers: 对header进行正则提取， key 为规则名称，value 为正则
  - body: 对response body 进行正则提取， key 为规则名称，value 为正则
- max_response_length: 为了确保整体扫描性能，防止超大body影响整体正则性能，这里对返回body进行大小过滤。**请视您的机器性能选择合适的值，这个数值调制过大可能会导致卡死**

#### 返回结果

- url



