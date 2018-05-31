# lnmp-dockerfiles

搭建lnmp环境

## 简介
用docker容器服务的方式搭建lnmp环境，易于维护、升级。使用前需了解Docker的基本概念，常用基本命令。
可以一条条命令执行docker命令来构建镜像，容器。这里推荐使用docker-compose来管理，执行项目，下面是使用流程。

相关软件版本：
- PHP 7.2
- MySQL 5.7 (root账号:root;密码5eNyjNf,成员账号:rageframe;密码:2589632147) [如何修改?](https://github.com/jianyan74/lnmp-dockerfiles/blob/master/docs/issue.md)
- Nginx 1.12
- Redis 4.0
- Mongo 3.6 (完全版)
- Elasticsearch latest (完全版)
- Rabbitmq latest (完全版)
- Memcached 1.5 (完全版)

用到的PHP扩展
- redis 4.0.0
- swoole latest
- memcached 3.0.4

> 注意:标注完全版的，通过切换full分支获得文件才能安装

#### 目录

目录 | 说明
---|---
--- app | 应用安装目录
--- data | mongo、mysql数据库文件存储
--- docs | 帮助文档
--- logs | nginx、mongo、mysql、php日志
--- sercices | 服务软件配置包
--- --- memcached | memcached配置及安装文件
--- --- mongo | memcached配置及安装文件
--- --- mysql | mysql配置及安装文件
--- --- nginx | nginx配置及安装文件
--- --- php | php配置及安装文件
--- --- redis | redis配置及安装文件
--- --- docker-composer.yml | docker配置执行文件


## 使用
#### 1.安装Docker，Docker-compose  
- Docker，详见官方文档：https://docs.docker.com/engine/installation/linux/docker-ce/centos/
- docker-compose，文档：https://docs.docker.com/compose/install/
- 镜像加速，参考[docker使用国内镜像](https://github.com/yeasy/docker_practice/blob/master/install/mirror.md)
       不然下载镜像速度会卡的你怀疑人生
- 常见问题，必看。参考[这里](https://github.com/jianyan74/lnmp-dockerfiles/blob/master/docs/issue.md)
```
sudo pip install -U docker-compose
```

#### 2.下载lnmp-dockerfiles
直接clone：
```
git clone https://github.com/jianyan74/lnmp-dockerfiles.git
# 如果需要完整版再执行 git checkout full
chmod -R 777 ./lnmp-dockerfiles/logs
cd lnmp-dockerfiles/services
```

#### 3.下载需要的拓展包
先下载好要使用的拓展包，如果编译出错要多次构建容器就可以省掉下载时间。
```
wget https://pecl.php.net/get/redis-4.0.0.tgz -O php/pkg/redis.tgz  
wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz -O php/pkg/libmemcached.tar.gz  
wget https://pecl.php.net/get/memcached-3.0.4.tgz -O php/pkg/memcached.tgz  
```

#### 4.docker-compose构建项目
进行docker-compose.yml所在文件夹：
执行命令：
```
docker-compose up
```  

如果没问题，下次启动时可以以守护模式启用，所有容器将后台运行：  
```
docker-compose up -d
``` 

使用 docker-compose 基本上就这么简单，Docker 就跑起来了，用 stop，start 关闭开启容器服务。  
更多的是在于编写 dockerfile 和 docker-compose.yml 文件。 

可以这样关闭容器并删除服务：
```
docker-compose down
```

#### 5. Demo站点搭建

进入app目录并克隆

```

cd ../app && git clone https://git.oschina.net/jianyan94/rageframe.git
cd rageframe
composer install
```

初始化项目(注意:以下关于用到php的最好都进入php容器内去执行，避免php版本对不上)

```
php init //然后输入0回车,再输入yes回车，注意如果想修改应用入口请先看入口修改文档
```

配置数据库信息

```
找到 common/config/main-local.php 并配置相应的信息
```

安装数据库

```
php ./yii migrate/up
```

域名解析

找到 `services/nginx/conf.d` 下的 demo.website.cnf 里修改第三行server_name
```
server_name [为你自己的域名]; 
```
注意重启一下nginx容器才能生效

## 问题反馈

在使用中有任何问题，欢迎反馈给我，可以用以下联系方式跟我交流

QQ群：[655084090](https://jq.qq.com/?_wv=1027&k=4BeVA2r)

## 引用

[zPhal-dockerfiles](https://github.com/ZpGuo/zPhal-dockerfiles)

## 学习文档
[Docker 配置详解](https://www.jianshu.com/p/2217cfed29d7)

[Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[Docker 微服务教程](http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)

