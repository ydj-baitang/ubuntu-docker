解决docker没法拉取镜像的问题。（liunx系统），如果是docker还没有安装可以下载docker.docx文件，里面有docker和docker-compose安装的详细过程。


1.在etc文件下创建一个docker目录，配置docker镜像加速，在docker目录下创建daemon.json文件

sudo mkdir /etc/docker           ##创建一个docker目录

vim /etc/docker/daemon.json      ##创建daemon.json文件

在文件daemon.json写入一下内容：

{

    "registry-mirrors": ["https://docker.rainbond.cc"]
}

sudo systemctl restart docker   ##重启docker
