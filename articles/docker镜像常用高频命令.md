
# Docker镜像常用高频命令
​

```bash

# 查看镜像（按创建时间排序）
docker images | head -n 6

# 查看正在运行中的容器（按创建时间排序）
docker ps | head -n 6

# 查看所有的容器（按创建时间排序）
docker ps -a

# 从镜像创建启动并进入容器命令行
docker run -it df5331382736  /bin/bash
docker run -it 镜像名:tag名  /bin/bash

# 启动已存在的容器
docker start c761dfc9d68c

# 进入已启动的容器
docker exec -it f3ce117e2910 /bin/bash

# 删除容器
docker rm -f df9db1ffb5c9

# 删除镜像
docker rmi 镜像名:tag名
docker rmi b50818b1c84a

# 拉取镜像
docker pull 远程镜像名:远程镜像tag

# 查看日志
docker logs --since="2021-09-02"  --tail=10 738d6927d946

# 查看日志：另一个方法
# 这个命令会打印日志路径，找到这个路径去把文件下载下来就行
docker inspect 05331c7b9f5d | grep -i logpath

# 打tag
docker tag old镜像名:oldtag  new镜像名:newtag

# 推镜像到仓库
docker push new镜像名:newtag

# 将镜像保存为tar.gz。这样可以获得更小体积的易于下载的镜像文件供传播
docker save 镜像名:tag  | gzip > 镜像名_tag名.tar.gz

# 把tar.gz加载成为本地镜像。save的时候是什么镜像名tag，load出来的时候就是什么名字，和tar包文件名无关。
docker load -i 镜像名_tag名.tar.gz

# 从指定镜像启动容器并开放端口，左边是本地端口，右边是容器端口
docker run -it -d -p 3307:3007 df5331382736

# 用root身份运行并进入容器
docker run -ti -u root c48d20a12832  /bin/bash

# 展示镜像构建时的所有命令执行历史
docker history 镜像名:tag名 --no-trunc

```
