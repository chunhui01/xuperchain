## 简介
为了支持以http方式调用xchain，实现了mini版的网关: http_gateway.go，该mini版的网关作为中间件的角色存在，用户的http请求直接转发给该网关，之后由该网关将http请求转换成grpc请求与xchain进行交互，并将交互结果转发给客户。
## 编译
go build -o xchain-httpgw gateway/http_gateway.go
## 部署
xchain-httpgw通常需要配合xchain节点一起部署，提供4个启动参数:

* `--http_endpoint`: 表示http服务侦听的端口，默认为8098；
* `--gateway_endpoint`: 表示xchain节点的rpc端口，默认为localhost:37101；
* `--enable_endorser`: 是否代理背书请求，默认为false；
* `--allow_cros`: 是否允许跨域请求，默认为false，在生产环境下**谨慎**使用。

一个启动命令举例：

> nohup ./xchain-httpgw --http_endpoint localhost:8098 --gateway_endpoint localhost:37101 &


## 使用
### 1.查询xuper链上高度为5的区块数据
命令: 
> curl http://localhost:8098/v1/get_block_by_height -d '{"bcname":"xuper", "height":5}'

结果如下:
![查询xuper链上高度为5的区块数据](https://github.com/ToWorld/xuperchain-image/blob/master/block_by_height.png)

### 2.查询bob的余额
命令: 
>curl http://localhost:8098/v1/get_balance -d '{"bcs":[{"bcname":"xuper"}],"address":"bob"}'

结果如下:
![查询bob的余额](https://github.com/ToWorld/xuperchain-image/blob/master/get_balance.png)

### 3.查询xuper链的状态
命令: 
>curl http://localhost:8098/v1/get_bcstatus -d '{"bcname":"xuper"}'

结果如下:
![查询xuper链的状态](https://github.com/ToWorld/xuperchain-image/blob/master/chainstatus.png)
