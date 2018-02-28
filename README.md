基于官方RabbitMQ镜像（rabbitmq:3-management），提供RabbitMQ Docker的docker-compose.yml文件。

### 单节点
```  
cd single-node
docker-compose up -d
```
虽然官方镜像提供了部分属性可以在生成容器时使用，但是若要更改其他属性还是不方便，所以单独编写了rabbitmq.conf文件，涉及基本属性、流控、网络分区处理等。默认用户名/密码为youdata/youdata（后续若需要在配置文件中对密码或其他属性进行加密，也是可以做到的）。

注：RabbitMQ在3.7.0之后采用了新的配置文件rabbitmq.conf，替换了rabbitmq.config，可支持类似properties文件的方式进行配置。

### 集群
```  
cd cluster
docker-compose up -d
```
网上大多数RabbitMQ Docker集群方案都是单机上完成的，单机多节点只是作为过渡。该方案实现了集群的配置以及使用HAProxy进行负载均衡（HAProxy的监控端口为8100，uri: /stats）。后续要实现镜像队列时，可按特定情况以类似下面的形式设定Policy：
```  
docker exec rabbit rabbitmqctl set_policy ha "." '{"ha-mode":"all"}'
```  


#### 参考：
http://josuelima.github.io/docker/rabbitmq/cluster/2017/04/19/setting-up-a-rabbitmq-cluster-on-docker.html
https://github.com/bijukunjummen/docker-rabbitmq-cluster
https://github.com/micahhausler/rabbitmq-compose
https://github.com/pardahlman/docker-rabbitmq-cluster
http://www.dockone.io/article/829