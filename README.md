# lnmp-dockerfiles

搭建lnmp环境

## 简介
用docker容器服务的方式搭建lnmp环境，易于维护、升级。使用前需了解Docker的基本概念，常用基本命令。
可以一条条命令执行docker命令来构建镜像，容器。这里推荐使用docker-compose来管理，执行项目，下面是使用流程。

相关软件版本：
- PHP 7.2
- MySQL 5.7 (root账号:root;密码5eNyjNf,成员账号:rageframe;密码:2589632147) [如何修改?](https://github.com/jianyan74/lnmp-dockerfiles/blob/master/docs/issue.md)
- Nginx 1.12
- Redis 3.2

用到的PHP扩展
- redis 3.1.4
- swoole 2.1

#### 建议先设置加速(重要)
不然下载速度会卡的你怀疑人生

[docker使用国内镜像](https://blog.csdn.net/yp090416/article/details/75107938)

#### 常见问题

https://github.com/jianyan74/lnmp-dockerfiles/blob/master/docs/issue.md

## 使用
#### 1.安装Docker，Docker-compose  
- Docker，详见官方文档：https://docs.docker.com/engine/installation/linux/docker-ce/centos/
- docker-compose，文档：https://docs.docker.com/compose/install/

```
sudo pip install -U docker-compose
```

#### 2.下载lnmp-dockerfiles
直接clone：
```
git clone https://github.com/jianyan74/lnmp-dockerfiles.git
```

#### 3.下载需要的拓展包
先下载好要使用的拓展包，如果编译出错要多次构建容器就可以省掉下载时间。
```
cd lnmp-dockerfiles/services

wget https://pecl.php.net/get/redis-3.1.6.tgz -O php/pkg/redis.tgz  
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

1、进入app目录并克隆

```

cd ../app && git clone https://git.oschina.net/jianyan94/rageframe.git
```

2、进入目录

```
cd rageframe
```

3、安装依赖

```
composer install
```
如果没有设置`composer`全局 也可直接输入 `php composer.phar install`

4、初始化项目

```
php init //然后输入0回车,再输入yes回车，注意如果想修改应用入口请先看入口修改文档
```

5、配置数据库信息

```
找到 common/config/main-local.php 并配置相应的信息
```

6、安装数据库

```
php ./yii migrate/up
```

7、域名解析

找到 `services/nginx/conf.d` 下的 demo.website.cnf 里修改第三行server_name
```
server_name [为你自己的域名]; 
```
注意重启一下nginx容器才能生效

### 问题反馈

在使用中有任何问题，欢迎反馈给我，可以用以下联系方式跟我交流

QQ群：[655084090](https://jq.qq.com/?_wv=1027&k=4BeVA2r)

## 引用

zPhal-dockerfiles： https://github.com/ZpGuo/zPhal-dockerfiles

## 学习文档

[Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[Docker 微服务教程](http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)

