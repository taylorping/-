# -
Jenkins+Docker+gitlab自动化集成环境

开发者向自己的gitlab网站提交了代码
jenkins通过定时任务检测到了代码有变成，执行自动化构建过程
jenkins在自动化构建脚本中调用docker命令将构建好的镜像push 私有镜像注册中心
同时，jenkins也可以直接执行remote shell启动构建好的容器
构建失败或者成功，可以及时将结果推送给相关人员，比如测试人员，安排测试
服务端可以手动通过docker命令，从镜像仓库中心拉取镜像，进行手动部署
我搭建的环境都是在本地，gitlab、jenkins、docker私有仓库都部署在本地，以下是操作步骤：

搭建docker私有仓库
使用docker拉取registry镜像，然后启动容器

#docker run -d -p 5000:5000 -v ~/docker-registry:/tmp/registry registry
这样就可以在本地运行一个私有镜像注册中心，通过镜像名称前缀127.0.0.1:5000可以将镜像推送到这个地址

搭建gitlab
拉取gitlab镜像，并启动

docker pull gitlab/gitlab-ce
sudo docker run --detach \
--hostname gitlab.bill.com \
--publish 443:443 --publish 80:80 --publish 22:22 \
--name gitlab \
--restart always \
--volume ~/gitlab/config:/etc/gitlab \
--volume ~/gitlab/logs:/var/log/gitlab \
--volume ~/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest

因为部署在本地，又指定了gitlab.bill.com作为域名，所以在/etc/hosts配置下，这样可以通过域名访问gitlab。











