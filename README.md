解决docker没法拉取镜像的问题。（liunx系统），如果是docker还没有安装可以下载docker.docx文件，里面有docker和docker-compose安装的详细过程。


1.在etc文件下创建一个docker目录，配置docker镜像加速，在docker目录下创建daemon.json文件

sudo mkdir /etc/docker           ##创建一个docker目录

vim /etc/docker/daemon.json      ##创建daemon.json文件

在文件daemon.json写入一下内容（两个中任选一个）：

第一个

{

    "registry-mirrors": ["https://docker.rainbond.cc"]
}

第二个

{

        "registry-mirrors": ["https://hub.atomgit.com"]

}

sudo systemctl restart docker   ##重启docker

然后再次拉去镜像尝试
sudo docker run hello-world   ##测试docker是否正常运行

sudo docker run -d -p 8080:80 nginx ##测试拉取和运行其他镜像

docker常用命令：

sudo docker run --name my-nginx -p 8080:80 -d nginx      ##运行储存库中的某个镜像

--name my-nginx：为启动的容器指定一个名字 my-nginx。

-p 8080:80：指定端口映射，将本地主机的 8080 端口映射到容器内的 80 端口。

-d：容器将在后台运行，而不是在终端中显示其输出。

sudo docker images                           ##查看本地存储的 Docker 镜像

sudo docker ps                               ##查看正在运行的 Docker 容器

sudo docker rmi <镜像ID或这名字>              ##删除本地存储的 Docker 镜像

sudo docker stop <容器的ID或者名字>           ##停止正在运行的 Docker 容器

sudo docker rm <容器的ID或者名字>             ##删除的 Docker 容器



