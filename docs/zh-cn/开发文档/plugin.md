开发流程

## 分类

以工作方式分类
- 容器中运行 （ demo: OneForAll ）
- 直接在 pokeball 内工作 (demo: Fofa)

>容器中运行的插件请在 `images` 目录下新建一个目录，目录名为插件名.  
目录下存在 `Dockerfile` (若为开源项目，将开源项目设为 submodule，若开源项目存在 Dockerfile ，可以将目录直接设置为submodule)       
然后在 `.github/workflows/build_images.yaml` 新增一个 `jobs`，能够确保自动build
> 
> 注：暂不接受无法在容器内且无法直接在pokeball内工作的插件 


以配置方式分类
- 注册时配置 （demo: PassiveXray）
- 运行时配置 （demo: Fofa）


## 开始


### 编写插件代码

明确了插件的工作方式，下面开始编写代码   
在`src/plugins`下新建文件，如新增 `Pikachu` 插件，新增文件`pikachu_plugin.go`
且需要实现以下方法


```go
package plugins

import (
	"google.golang.org/grpc"
	"pokemonscan-pokeball/src/proto/pokeball"
)

type PikachuPlugin struct {
	Name string
}


func (p *PikachuPlugin) Register(conn grpc.ClientConnInterface, pluginConfig string) error {
	p.Name = "Pikachu"
	return nil
}

func (p *PikachuPlugin) Run(taskId int32, pluginConfig string) error {
	
	return nil
}

func (p *PikachuPlugin) GetName() string {
	return p.Name
}

func (p *PikachuPlugin) GetResult(taskId int32) (*pokeball.ReportInfoArgs, *pokeball.ReportVulArgs, error) {

	resArgs := &pokeball.ReportVulArgs{}

	return nil, resArgs, nil
}

func (p *PikachuPlugin) GetListenAddress(fromContainer bool) string {
	return ""
}

func (p *PikachuPlugin) UpdateConfig(pluginConfig string) error {
	return nil
}
```

#### Register

插件注册时候下发的配置，这里需要将`pluginConfig` Unmarshal 为 `PikachuConfig` , 并根据参数启动工具

#### Run

插件允许时候下发的配置，这里需要将`pluginConfig` Unmarshal 为 `PikachuConfig` , 并根据参数启动工具
并将结果存储至本地

> 常用功能代码块
> 
> - 获取存储结果文件目录
> ```go
> resultDir := utils.GetPluginTmpDir(p.Name, "result")
> pikachuResultFile := path.Join(resultDir, fmt.Sprintf("pikaqiu_qs-%d", taskId))
>```
> - 获取插件运行容器名
> ```go
> containerName := utils.GetPluginContainerName(p.Name, taskId)
> ```
> - 写入配置文件
> ```go
> 	if config.RadConfigFile != "" {
>		var err error
>		radConfigFile, err = utils.WriteFileFromBase64(configDir, fmt.Sprintf(radConfigFileFormat, taskId), config.RadConfigFile)
>		if err != nil {
>			radConfigFile = ""
>		}
>	} 
> ```
> - 运行容器
> ```go
> 	containerConfig := &container.Config{
>		Image: plugin_proto.ENScanImageName,
>		Cmd: []string{"--json-output", "-o", fmt.Sprintf("/app/res/pikaqiu_qs-%d", taskId),
>			"-n", config.Target, "-type", config.Type,
>		},
>		Hostname: containerName,
>	}
>
>	hostConfig := &container.HostConfig{AutoRemove: true,
>		ExtraHosts: []string{"host.docker.internal:host-gateway"},
>		Mounts: []mount.Mount{
>			{
>				Type:   mount.TypeBind,
>				Source: resultDir,
>				Target: "/app/res",
>			},
>		},
>	}
>
>	err := docker.WaitForRun(containerConfig, hostConfig, nil, containerName)
>	if err != nil {
>		return err
>	}
> ```
> - 获取代理 url
> ```go
> 	downstreamProxyUrl := ""
>
>	if config.DownstreamPlugin != "" {
>		if downstreamProxyPlugin, ok := conf.PokeballPlugins[config.DownstreamPlugin]; ok {
>			// 存在
>			downstreamProxyUrl = downstreamProxyPlugin.GetListenAddress(true)
>		}
>
>	}
>```

#### GetResult

上报结果

#### UpdateConfig

更新配置时执行

#### GetListenAddress

前文提到插件有两种工作状态，容器内/插件内，当涉及到代理相关的时候，对于上游代理地址是不同的。
若当前插件是容器内运行（如PassiveXray）
```go
func (p *PikachuPlugin) GetListenAddress(fromContainer bool) string {
	containerName := utils.GetPluginContainerName(p.Name, 0)

	if fromContainer {
		return fmt.Sprintf("%s:%d", containerName, p.Config.ListenPort)
	}
	return fmt.Sprintf("127.0.0.1:%d", p.Config.ListenPort)
}
```

若当前插件是插件内执行 （如 Recorder）
```go
func (p *PikachuPlugin) GetListenAddress(fromContainer bool) string {
	if fromContainer {
		return fmt.Sprintf("host.docker.internal:%d", p.Config.ListenPort)
	}
	return fmt.Sprintf("127.0.0.1:%d", p.ListenPort)
}
```



### 配置参数

在 `src/proto/proto_struct/plugin` 新建 `pikachu.go`

```go
package plugin

const PikachuPluginName = "Pikachu"

type PikachuConfig struct {
	Target string `json:"target"`
	Type   string `json:"type"`
}
```


### manager 内注册

在 `src/manager/plugin_manager.go` 注册插件 

```go
conf.PokeballPlugins = map[string]pokeball.Plugin{
    plugin_proto.PassiveXrayPluginName: &plugins.PassiveXrayPlugin{Name: plugin_proto.PassiveXrayPluginName},
    .......
    .......
    plugin_proto.PikachuPluginName:  &plugins.PikachuPlugin{Name: plugin_proto.PikachuPluginName}
}
```

## 测试

写一个对应的配置文件 `config.json`

```json
{
  "plugins_config": {
    "target": "xxx.com",
    "type": "pika"
  }
}
```

执行pokeball时候增加命令行参数
```bash
./pokeball -c config.json single
```