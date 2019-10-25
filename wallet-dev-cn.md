# FSN钱包开发指南

本文档主要描述，交易所、矿池、钱包等在对接FSN的开发过程中涉及到充值、提现、转账、查询等接口，以及如何一键部署FSN节点。

## 部署FSN节点

FSN节点支持两种部署方法：
1、docker镜像一键部署，镜像地址：https://hub.docker.com/u/fusionnetwork
2、源码编译部署

### 1. docker一键部署

`
bash -c "$(curl -fsSL https://raw.githubusercontent.com/FUSIONFoundation/efsn/master/QuickNodeSetup/fsnNode.sh)"

`

部署过程中如果选择挖矿节点需要输入keystore文件和password，详细请参考：https://fusionnetworks.zendesk.com/hc/en-us/categories/360001967614-Staking-On-Fusion-MainNet

### 2. 源码编译部署

同步代码后`make efsn`编译部署

`git clone https://github.com/FUSIONFoundation/efsn.git`

编译源码(golang > 1.11)：

`cd efsn && make efsn`

运行节点：

`./build/bin/efsn console`

作为后台同步节点开放RPC接口的运行参数如下：

`nohup ./build/bin/efsn --datadir ./node1/  --gcmode=archive --rpc --rpcaddr 0.0.0.0 --rpcapi net,fsn,eth,web3 --rpcport 9001 --rpccorsdomain "*" &`

作为同步节点能够查询所有历史数据需要打开`--gcmode=archive`参数，在770000块高度是占用硬盘空间超过100G，采用此模式需要提前准备服务器存储空间（建议>300G）。非archive模式1G左右，但无法查询一些历史数据。

## 接口开发

### 查询余额

### 充值识别

### 提现交易

