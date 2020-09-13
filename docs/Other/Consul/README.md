# Consul

[^_^]:![Consul](https://tozii.github.io/Asset/document/images/consul.jpg)

**Github**
> [Github：https://github.com/hashicorp/consul](https://github.com/hashicorp/consul)

**介绍**

&emsp;&emsp;Consul是一种服务网格解决方案，提供具有服务发现，配置和分段功能的全功能控制平面。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全服务网格。Consul需要数据平面并支持代理和本机集成模型。Consul附带一个简单的内置代理，因此一切都可以开箱即用，但也支持第三方代理集成，如Envoy。<br/>
**Consul的主要特点是：**
- 服务发现：Consul的客户可以注册服务，例如 api或mysql，并且其他客户可以使用Consul来发现给定服务的提供者。使用DNS或HTTP，应用程序可以轻松找到它们所依赖的服务。<br/>
- 运行状况检查：Consul客户端可以提供任意数量的运行状况检查，这些检查与给定服务（“是Web服务器返回200 OK”）或本地节点（“内存利用率低于90％”）相关联。运营商可以使用此信息来监控群集运行状况，服务发现组件使用此信息将流量路由远离不健康的主机。<br/>
- KV存储：应用程序可以将Consul的分层键/值存储用于任何目的，包括动态配置，功能标记，协调，领导者选举等。简单的HTTP API使其易于使用。<br/>
- 安全服务通信：Consul可以为服务生成和分发TLS证书，以建立相互的TLS连接。 意图 可用于定义允许哪些服务进行通信。可以使用可以实时更改的意图轻松管理服务分段，而不是使用复杂的网络拓扑和静态防火墙规则。<br/>
- 多数据中心：Consul支持多个数据中心。这意味着Consul的用户不必担心构建额外的抽象层以扩展到多个区域<br/>

**使用**
> [.NET Core微服务实施之Consul服务发现与治理 — dotnetgeek](https://www.cnblogs.com/waynechan/p/9354909.html)

[官方操作文档](https://www.consul.io/commands)

windows：下载consul压缩包到本地，然后解压进入consul.exe文件所在目录，打开命令行使用 `consul` 或 `consul -h` 命令可以查看consul所有操作命令。<br/>
使用 `consul [command] -h` 可以查看指定命令帮助，如： `consul agent -h` 查看agent详细命令

使用 `consul catalog nodes` 命令列出所有已知节点

`consul catalog datacenters` 列出所有数据中心

`consul catalog nodes -service=redis` 列出所有提供特定服务的节点

`consul catalog services` 列出所有服务

`consul catalog services -node=worker-01` 列出节点上的所有服务

**consult快照**

命令： `consul snapshot [command]`

该snapshot命令具有用于保存，还原和检查Consul服务器状态以进行灾难恢复的子命令。这些是原子的时间点快照，其中包括键/值条目，服务目录，准备好的查询，会话和ACL

停止代理 `consul leave`

如：`consul snapshot save backup.snap` 创建快照并命名为backup.snap

`consul snapshot restore backup.snap` 恢复指定快照

`consul snapshot agent [可选参数]` 命令启动一个守护进程，更加参数可以长时间运行并更加参数定时保存快照

快照选项

`-interval`-以单位后缀作为时间执行快照的时间间隔，可以是“ s”，“ m”，“ h”，表示秒，分钟或小时。如果提供0，则代理将创建单个快照，然后退出，这对于通过批处理作业运行快照很有用。默认为“ 1h”

`-retain`-要保留的快照数量。拍摄每个快照后，最早的快照将开始删除，以便最多保留这么多快照。如果将其设置为0，则代理将不会执行此操作，快照将永远累积。默认为30。

例子

不带任何参数运行代理将运行一个长时间运行的守护进程，该进程将执行领导者选举以进行高可用性操作，通过运行状况检查向Consul服务发现进行注册，每小时进行一次快照，保留最后30个快照，并将快照保存到当前工作目录：

`consul snapshot agent`

[官方入门教程](https://learn.hashicorp.com/tutorials/consul/get-started-agent)

在生产中，您将以服务器或客户端模式运行每个Consul代理。每个Consul数据中心必须至少具有一个服务器，该服务器负责维护Consul的状态。这包括有关其他Consul服务器和客户端，可用于发现的服务以及允许哪些服务与哪些其他服务进行通信的信息。

非服务器代理以客户端模式运行。客户端是注册服务，运行状况检查并将查询转发到服务器的轻量级进程。客户端必须在Consul数据中心中运行服务的每个节点上运行，因为客户端是有关服务运行状况的真实来源。

!> 开发模式启动consul代理 `-dev` 生产模式勿用
`consul agent -dev

Consul使用您的主机名作为默认节点名。如果您的主机名包含句点，则对该节点的DNS查询将不适用于Consul。为避免这种情况，请使用-node标志显式设置节点的名称。`

`consul agent -dev -node [主机名]`

查询数据中心成员
`consul members` 或 `consul members -detailed` 查询更多

**启动代理参数**

- `-bind` 内部集群通信应绑定的地址
- `-ui` 启用内置的Web UI服务器和所需的HTTP路由
- `bootstrap-expect` 此标志提供数据中心中预期的服务器数量。要么不提供此值，要么该值必须与集群中的其他服务器一致。提供后，Consul将等待直到指定数量的服务器可用为止，然后引导群集。这样可以自动选举最初的领导者。不能与旧-bootstrap标志一起使用。该标志需要-server模式。
- `-config-file` 要加载的配置文件
- `-config-dir` 要加载的配置文件目录
- `-data-dir` 此标志为代理提供了一个用于存储状态的数据目录。这是所有代理程序所必需的。该目录在重新引导后应是持久的。这对于在服务器模式下运行的代理来说尤其重要，因为它们必须能够保持群集状态
- `-dev` 启用开发服务器模式
- `-http-port` 监听的HTTP API端口。这将覆盖默认端口8500
- `-https-port` 用于侦听的HTTPS API端口
- `-log-file` 将所有Consul代理日志消息写入文件。该值用作日志文件名的前缀。当前时间戳将附加到文件名。如果该值以路径分隔符结尾，consul- 则将应用于该值。如果文件名缺少扩展名，.log 则会附加。例如，设置log-file为/var/log/会导致日志文件路径为/var/log/consul-{timestamp}.log。log-file可以-log-rotate-bytes与-log-rotate-duration结合使用 ， 以获得细粒度的日志轮换体验。
- `-join` 启动时要加入的另一个代理的地址
- `-node` 集群中此节点的名称
- `-server` 此标志用于控制代理是处于服务器还是客户端模式。提供后，代理将充当Consul服务器。每个Consul群集必须至少具有一台服务器，并且每个数据中心最好不超过5台。所有服务器都参与Raft共识算法，以确保事务以一致的，可线性化的方式发生。事务会修改群集状态，群集状态将在所有服务器节点上维护，以确保在节点出现故障时可用。服务器节点还与其他数据中心中的服务器节点一起参与WAN闲话池。服务器充当其他数据中心的网关，并根据需要转发流量。

> 在多代理Consul数据中心中，每个服务都将向其本地Consul客户端注册，并且客户端会将注册转发给Consul服务器，该服务器维护服务目录。（此处consul服务器为使用`-server`参数启动的代理）

**同个服务器部署多个consul代理**

第一个服务代理`consul agent -server -ui -data-dir ./tmp -bootstrap-expect 1 -config-dir ./consul.d -node consul1`

-config-dir代理启动需要的配置文件,由于同一个服务器启动两个代理，默认端口会冲突，所有在配置文件里面修改了端口（只有http端口使用默认8500没有修改）
```json
{
    "ports": {  
        "http": 8500,
        "dns": 8601,
        "grpc": 8401,
        "serf_lan": 8311,
        "serf_wan": 8312, 
        "server": 8310
      }
}
```
第二个服务代理`consul agent -server -data-dir ./tmp2 -http-port 8001 -node consul2 -bootstrap-expect 1` 使用-http-port重新指定了端口，避免和第一个代理冲突
```shell
D:\consul>consul members
consul1
consul2
D:\consul>consul catalog nodes
consul1
consul2
```
> consul 多个数据中心数据不会同步，所以在部署consul集群时每个数据中心至少部署3-5个consul服务器，consul客户端不限数量，多个consul服务器用于数据保存，在leader节点发生故障时不影响服务正常运行（因为每个consul服务器都会保存数据中心数据）