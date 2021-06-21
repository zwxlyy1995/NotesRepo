# docker hub
账号：randomless
密码：randomless

# docker 19.03安装
docker 19.03安装在ubuntu下安装(可使用显卡)　https://blog.csdn.net/li_ellin/article/details/107180516

# docker管理工具

最佳实践：Portainer + lazydocker

Portainer安装: https://blog.csdn.net/u011781521/article/details/80469804   http://10.1.17.83:9000/#/home
lazydocker快捷键: https://github.com/jesseduffield/lazydocker/blob/master/docs/keybindings/Keybindings_en.md


# docker vscode开发
配置方法 https://zhuanlan.zhihu.com/p/80099904

1. 运行容器、挂载目录到workspace
```bash
nvidia-docker run -it -p 50001:22 --ipc=host --name="tf" -v /media/disk/zzt:/workspace tensorflow/tensorflow:1.14.0-gpu-py3  /bin/bash

docker run -it --gpus  'device=1' -p 3005:22 --ipc=host --name="extract_nightly" -v /media/dyp/S3/docker/extract:/workspace 16c127d192a2  /bin/bash
```
2. 容器内安装ssh-serve
```bash
apt-get update
apt-get install -y openssh-server --allow-unauthenticated
# --allow-unauthenticated

mkdir /var/run/sshd
echo -e '\n \n    --------You are Changing Password Of root:passwd-------   \n \n'  
passwd
# 这里使用你自己想设置的用户名和密码，但是一定要记住！
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/'  /etc/ssh/sshd_config
# sed -i 's/原字符串/新字符串/' 　　/home/1.txt 　　sed文本替换
# sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
# echo "export VISIBLE=now" >> /etc/profile

service ssh restart

```
3. 在服务器（宿主机）上测试新建的docker容器中哪个端口转发到了服务器的22端口
```bash
docker port [container_name] 22

# 如果前面的配置生效了，你会看到如下输出
# 0.0.0.0:8022
```

4. 在本地测试能否用SSH连接到远程docker：
```bash
ssh-keygen -f "/home/neo/.ssh/known_hosts" -R "[10.1.17.11]:50002" 
ssh root@[your_host_ip] -p 50001

ssh root@10.1.17.83  -p 50002
ssh root@10.1.17.11  -p 50002


# 出现WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!　
# 解决：ssh-keygen -f "/home/neo/.ssh/known_hosts" -R "[10.1.17.83]:8022" 

# 密码是你前面自己设置的
```

**sh脚本**
```sh
#!/bin/bash

echo "docker image name/id: $1";
docker_image=$1;
echo "host port: $2";
host_port=$2;

container_id=$(docker run -p ${host_port}:22 -dit ${docker_image} /bin/bash);
echo "container id: ${container_id}";
docker exec ${container_id} /etc/init.d/ssh start;

echo "enter password: ";
docker exec -it ${container_id} passwd;

```


## 在服务器83搭建tensorflow1.10的docker


pull时候
run容器时候：`nvidia-docker` run -it --rm 0b4ceed1758b bash

## trouble-shot
解决docker中的容器无法使用中文的问题  https://www.jianshu.com/p/6fe582ba6d3d

##　概念
docker学习——什么是image、container、service、swarm　https://blog.csdn.net/dhaiuda/article/details/82782189

## 命令

gpu命令 新版docker代替nvidia-docker
docker run --gpus '"device=1,2"' nvidia/cuda:10.0-base nvidia-smi


如果想退出容器但又不想让容器停止，使用Ctrl+P+Q即可

删除所有已经停止的容器：
docker rm $(docker ps -a -q)


拷贝本地文件到容器
docker cp 本地路径 容器长ID:容器路径

docker cp /Users/xubowen/Desktop/auto-post-advance.py 38ef22f922704b32cf2650407e16b146bf61c221e6b8ef679989486d6ad9e856:/root/auto-post-advance.py

docker cp -r /data4/dockerPack/ExtractTown/ExtractTown 5f98fb73e850:/workspace

Docker保存修改后的镜像 https://blog.csdn.net/bugzeroman/article/details/82457842

连接正在运行的容器
docker exec -it 775c7c9ee1e(容器ID) /bin/bash

保存镜像为文件
docker save -o extract.tar extract:v0.002

# gdal安装
关于Ubuntu18.04 python3.6安装GDAL采坑实录 https://blog.csdn.net/u010329292/article/details/90242235
export PYTHONPATH=/usr/local/lib/python3.5/dist-packages/GDAL-2.4.1-py3.5-linux-x86_64.egg/osgeo：$PYTHONPATH

/usr/local/lib/python3.5/dist-packages/GDAL-2.4.1-py3.5-linux-x86_64.egg/osgeo