# docker-rocketmq 镜像

镜像将不再根据base镜像生成server、broker镜像，统一使用base镜像，两者区别只是调用的启动文件不同

# 一键部署 docker-compose
案例 版本 4.8.0

```SHELL
cd rmq
chmod +x  start.sh
./start.sh
```

访问浏览器
```SHELL
localhost:8180
```

>注意 如果你的微服务或者项目在开发的时候没有放入`docker`中或者与`rocketmq`容器不能直接用IP访问，
那么请把`broker.conf`中的 `#brokerIP1=192.168.0.253` 前面`#`号去掉，并且把后面的`IP地址`改成你的`rocketmq`容器宿主机`IP地址`,
否则报 `com.alibaba.rocketmq.remoting.exception.RemotingConnectException: connect to <172.0.0.120:10909> failed`
>配置文件 在 `rmq/rmq/brokerconf` 目录下

# rocketmq启动
## server
### server 无日志目录映射
```bash
docker run -d \
      --name rmqnamesrv \
      -e "JAVA_OPT_EXT=-Xms512M -Xmx512M -Xmn128m" \
      -p 9876:9876 \
      rookiefly/rocketmq:latest\
      sh mqnamesrv
```
### server 有日志目录映射
```bash
docker run -d -v $(pwd)/logs:/home/rocketmq/logs \
      --name rmqnamesrv \
      -e "JAVA_OPT_EXT=-Xms512M -Xmx512M -Xmn128m" \
      -p 9876:9876 \
      rookiefly/rocketmq:latest\
      sh mqnamesrv
```
## broker
### broker 无 目录映射
```bash
docker run -d \
      --name rmqnamesrv \
      -e "JAVA_OPT_EXT=-Xms512M -Xmx512M -Xmn128m" \
      -p 9876:9876 \
      rookiefly/rocketmq:latest\
      sh mqbroker -c /home/rocketmq/conf/broker.conf
```
### broker 目录映射
```bash
docker run -d  -v $(pwd)/logs:/home/rocketmq/logs -v $(pwd)/store:/home/rocketmq/store \
      -v $(pwd)/conf:/home/rocketmq/conf \
      --name rmqbroker \
      -e "NAMESRV_ADDR=rmqnamesrv:9876" \
      -e "JAVA_OPT_EXT=-Xms512M -Xmx512M -Xmn128m" \
      -p 10911:10911 -p 10912:10912 -p 10909:10909 \
      rookiefly/rocketmq:latest\
      sh mqbroker -c /home/rocketmq/conf/broker.conf
```

注意（重要的事说3遍）

注意（重要的事说3遍）

注意（重要的事说3遍）

>如果你的微服务没有使用`docker`,那么需要把`/etc/rocketmq/broker.conf` 配置文件中的`brokerIP1=192.168.0.253` 这个启用，IP 地址填写 你docker 所在 宿主机的IP ，否则报错


## console
来自
https://hub.docker.com/r/styletang/rocketmq-console-ng/

```SEHLL
docker run --name rmqconsole --link rmqserver:rmqserver \
-e "JAVA_OPTS=-Drocketmq.namesrv.addr=rmqserver:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" \
-p 8180:8080 -t styletang/rocketmq-console-ng
```

浏览器访问
```SEHLL
localhost:8180
```