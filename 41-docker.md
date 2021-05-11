## Docker

### 安装

```shell
# 卸载当前安装
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 所需安装包
yum install -y yum-utils

# 设置镜像(阿里云)
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新yum索引
yum makecache fast

# 安装docker相关的东西 ce 社区版 
yum install docker-ce docker-ce-cli containerd.io

# 启动docker
systemctl start docker

# hello-world
docker run hello-world
```

- 卸载

  ```shell
   sudo yum remove docker-ce docker-ce-cli containerd.io
  
   sudo rm -rf /var/lib/docker
   sudo rm -rf /var/lib/containerd
  ```

- 阿里云镜像加速

  ```shell
  sudo mkdir -p /etc/docker
  sudo tee /etc/docker/daemon.json <<-'EOF'
  {
    "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
  }
  EOF
  sudo systemctl daemon-reload
  sudo systemctl restart docker
  ```

- 原理

  ###### docker是怎么工作的

  Docker是一个CS结构的系统 Docker的守护进程运行再主机上 通过socket从客户端访问

  DockerServer接收到DockerClient的命令 就会执行这个命令

  ![image-20210507182353322](C:\Users\cyrex\AppData\Roaming\Typora\typora-user-images\image-20210507182353322.png)

  ###### docker为什么比vm快

  1. docker有着比虚拟机更好的抽象层

  2. docker里用的是宿主机的内核 vm需要的是guest os

     |            | Docker容器  | LXC         | VM         |
     | ---------- | ----------- | ----------- | ---------- |
     | 虚拟化类型 | OS虚拟化    | OS虚拟化    | 硬件虚拟化 |
     | 性能       | =物理机性能 | =物理机性能 | 5%-20%损耗 |
     | 隔离性     | NS隔离      | NS隔离      | 强         |
     | QoS        | Cgroup弱    | Cgroup弱    | 强         |
     | 安全性     | 中          | 差          | 强         |
     | GuestOS    | 只支持Linux | 只支持Linux | 全部       |

### 常用命令

- 帮助命令

```shell
# 版本信息
docker version
# 镜像信息
docker info
docker 命令 --help # 万能命令

```

- 镜像命令

  - 查看所有镜像

    ```shell
    docker images
    # 镜像仓库源    标签			id			创建时间		大小
    REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
    hello-world   latest    d1165f221234   2 months ago   13.3kB
    
    
    Options:
      -a, --all             所有镜像
      -q, --quiet           只显示镜像
    ```

  - 搜索镜像

    ```shell
    docker search mysql
    
    NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
    mysql                             MySQL is a widely used, open-source relation…   10837     [OK]       
    # 可选项
    --filter=STARS=3000
    ```

  - 拉取镜像

    ```shell
    docker pull
    
    # Options:
      -a, --all-tags                Download all tagged images in the repository
          --disable-content-trust   Skip image verification (default true)
          --platform string         Set platform if server is multi-platform capable
      -q, --quiet                   Suppress verbose output
    
    ```

  - 删除镜像

    ```shell
    docker rmi -f 镜像id
    
    docker rmi -f $(docker images -aq) # 查出所有镜像id并递归删除
    ```

- 容器命令

  - 新建容器并启动

    ```shell
    docker run [可选命令] imagesid
    
    # option 
    --name="name"  容器名字
    -d 			   后台方式运行
    -it            使用交互方式运行,进入容器查看内容
    -p			   指定容器的端口 -p 3306:3306
    	-p IP:主机端口:容器端口 
    	-p 主机端口:容器端口 (常用)
    	-p 容器端口
    	
    
    docker run -it centos /bin/bash
    
    docker run -d --name nginx01 -p 3344:80 nginx
    curl localhost:3344 # nginx 启动并测试
    ```

  - 退出容器

    ```shell
    exit # 停止容器
    
    ctrl + p + q # 不停止退出
    ```

  - 列出正在运行容器

    ```shell
    docker ps [可选]
    # options
    -a  		列出正在运行的容器
    -n=? 		最近创建的?个容器
    -q 			只显示容器编号
    ```

  - 删除容器

    ```shell
    docker rm 容器id  # 删除指定 不能删除正在运行的 强制删除 -f
    
    docker rm -f $(docker ps -aq) # 删除所有
    
    docker ps -a -q|xargs docker rm # 删除所有容器
    ```

  - 启动/停止容器

    ```shell
    docker start 容器id    # 启动
    docker restart 容器id  # 重启
    docker stop 容器id 	 # 停止
    docker kill 容器id     # 强制停止
    ```

- 常用的其他命令

  - 后台启动

    ```shell
    docker run -d 镜像名
    
    # 问题 ps centos停止了
    # docker容器使用后台运行 必须要有一个前台进程 没有前台应用就会自动停止
    ```

  - 查看日志

    ```shell
    docker logs
    
    # 自己写一段shell 方便查看日志
    docker run -d centos /bin/sh -c "while true;do echo hello docker;sleep 1;done"
    
    docker ps
    
    # 显示日志
    -tf 		  # 显示日志
    --tail number # 显示最后number行日志
    ```

  - 查看进程进程

    ```shell
    docker top 容器id
    
    UID        PID     PPID      C   STIME    TTY    TIME     CMD
    polkitd   5788     5768     0    10:43     ?   00:00:03   mysqld
    
    ```

  - 查看容器的源信息

    ```shell
    docker inspect 容器id
    ```

  - 进入当前正在运行的容器

    ```shell
    docker exec -it 容器id bashShell
    
    docker attach 容器id
    
    # 区别
    docker exec   # 进入容器后开启一个新的终端 可以在里面操作 (常用)
    docker attach # 进入容器正在执行的终端 不会启动新的进程 
    ```

  - 从容器拷贝文件到主机

    ```shell
    docker cp 容器id:容器内路径 主机路径
    
    cd /home # 容器内的cd
    touch test.java # 新建文件
    exit # 退出
    
    docker cp b30:/home/test.java /home 
    
    # 拷贝是手动过程 后期可以使用 -v 卷的技术实现主机和容器的同步
    ```





















