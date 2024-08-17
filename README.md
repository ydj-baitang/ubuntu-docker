Ubuntu22.04上部署Docker容器和安装Docker compose
解决docker官网国内访问不了，没法拉取镜像的问题。
虽然docker官方访问不了，但是还有很多仍在服务的镜像源供我们使用。
1.	安装步骤
sudo apt update && sudo apt upgrade   ##更新ubuntu系统
我们可以从阿里云开源镜像网站下载所需的版本：（https://mirrors.aliyun.com/docker-ce/linux/static/stable/x86_64/）
wget https://mirrors.aliyun.com/docker-ce/linux/static/stable/x86_64/docker-23.0.6.tgz
tar -zxvf docker-23.0.6.tgz      ##解压下载好的docker
cp docker/* /usr/local/bin      ##将docker下的所有文件拷贝到bin目录

2.在etc文件下创建一个docker目录，配置docker镜像加速，在docker目录下创建daemon.json文件
sudo mkdir /etc/docker           ##创建一个docker目录
vim /etc/docker/daemon.json      ##创建daemon.json文件
在文件daemon.json写入一下内容：
{
    "registry-mirrors": ["https://docker.rainbond.cc"]
}

3.在文件/etc/systemd/system/下创建docker.service，用于启动docker服务（docker.service内容如下）：
sudo vim /etc/systemd/system/docker.service
4.docker.service内容如下：
[Unit]
Description=Docker Application Container Engine
After=network.target  
Wants=network-online.target  

[Service]
Type=notify
# 启动服务的命令，指定 Docker 守护进程的路径
ExecStart=/usr/local/bin/dockerd
# 重新加载服务时执行的命令，发送 HUP 信号重新加载配置
ExecReload=/bin/kill -s HUP $MAINPID
# 停止服务时终止主进程及其所有子进程
KillMode=process
# 服务失败后自动重启
Restart=on-failure
# 服务失败后自动重启的等待时间，设置为 5 秒
RestartSec=5s
# 设置服务可以打开的最大文件描述符数
LimitNOFILE=1048576
# 设置服务可以创建的最大进程数
LimitNPROC=1048576
# 设置挂载标志为 shared，表示挂载点的共享状态
MountFlags=shared
[Install]
# 指定服务应启用在 multi-user.target 中
WantedBy=multi-user.target

5.配置完后重新加载服务，查看docker状态： 
sudo systemctl daemon-reload      
sudo systemctl restart docker        ##重新加载docker服务
sudo systemctl status docker        ##查看docker状态
sudo systemctl enable docker        ##服务启动是启动（开机自启）
 
如图说明docker服务成功启动。

6.测试docker是否正常运行
sudo docker run hello-world
 
7.拉取和运行其他镜像
docker run -d -p 8080:80 nginx
 
8.查看docker镜像和容器
docker images       ##查看本地存储的 Docker 镜像
docker ps           ##查看正在运行的 Docker 容器
 
 

9.安装Docker Compose
Docker Compose 是一个可用于定义和运行多容器 Docker 应用程序的工具。使用 Compose，你可以使用 Compose 文件来配置应用程序的服务。然后，使用单个命令，你可以从配置中创建和启动所有服务。
10.从github（https://github.com/docker/compose/releases/）上下载安装Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.29.1/docker-compose-$(uname -s)-$(uname -
m)" -o /usr/local/bin/docker-compose
赋予文件docker-compose可执行的权限：
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version          ##查看版本信息
