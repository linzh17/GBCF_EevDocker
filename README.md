# GBCF_EevDocker
该镜像 适用于本地测试使用，可以在linux和windows 两种平台部署，下面会列出部署的方法

## 克隆项目
```
git clone https://github.com/scnu-dil/GBCF_EnvDocker.git
```

## 安装docker 

* linux 

    查看官方安装文档
* windows

    从docker 官网安装docker desktop 

## 构建镜像
```
cd /GBCF_EnvDocker/environment 

docker build -t block:v1 .

```
镜像的构建时间较长，需要30到40分钟，取决于网速，如果镜像构建出现报错，可以尝试再建立一次

镜像建立完成后可以通过以下命令进行查询 ，如果有名为block的镜像，说明镜像建立成功
```
docker image ls
```
## 构建容器

### linux
构建容器
```
docker run -it --name=block block:v1 /bin/bash
```
运行上述命令，则建立名为block的容器并且进入容器，可以通过以下命令查询建立的容器
```
docker ps -a
```
进入容器后，进入根目录并克隆项目
```
cd ~

git clone https://github.com/scnu-dil/GeneralBlockchainFramework.git
```
拷贝区块链服务器证书等相关文件到GeneralBlockchainFramework项目中
```
cp /usr/local/src/sdk.key ~/GeneralBlockchainFramework/contracts/bin/　
cp /usr/local/src/sdk.crt ~/GeneralBlockchainFramework/contracts/bin/
cp /usr/local/src/node.crt ~/GeneralBlockchainFramework/contracts/bin/
cp /usr/local/src/node.key ~/GeneralBlockchainFramework/contracts/bin/
cp /usr/local/src/ca.crt ~/GeneralBlockchainFramework/contracts/bin/
cp /usr/local/src/applicationContext.xml  ~/GeneralBlockchainFramework/contracts/bin/
```
！！！注意 区块链服务器证书等相关文件，需要提前从服务器拷贝，放在GBCF_EnvDocker/environment/ 下，这样镜像构建的时候，会自动把这些文件放入容器中/usr/local/src中,如果有更换区块链服务器，请注意更换GBCF_EnvDocker/environment/  下的 *node.crt,node.key,ca.crt,sdk.crt,sdk.key, applicationContext.xml* 文件

修改GeneralBlockchainFramework中 ~/GeneralBlockchainFramework/contracts/client_config.py 中的一些配置

```
vim ~/GeneralBlockchainFramework/contracts/client_config.py 

```
修改文件中 区块链服务器两项host为服务器公网ip
```
remote_rpcurl = "http://47.xxx.xxx.169:8545"  
channel_host = "47.xxx.xxx.169"  
```

运行测试
设置python环境
```
cd ~/GeneralBlockchainFramework

export PYTHONPATH=$PYTHONPATH:/root/GeneralBlockchainFramework 

```
测试 TianwenTest.py

```
python ./test/TianwenTest.py
```
运行接口程序
注意 这里 需要进入 router.py 修改
 app.run(debug=True)为 app.run(host='0.0.0.0',debug=True)
```
python ./API/router.py

```
测试接口
```
docker inspect block
```
可以看到block容器的ip地址为172.17.0.2，可以通过这个ip地址对接口进行访问

### windows 
只有部分操作不一样 这里只列出操作不一样的地方

构建容器

```
docker run -it --name=block -p5000:5000 block:v1 /bin/bash
```
这里应该会弹出防火墙警告，请允许操作

测试接口 

可以通过本地127.0.0.1:5000对接口进行访问


## docker 容器操作
```
#启动容器
docker start block

#访问容器
docker attach  block
```