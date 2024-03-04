## ivc-gpu lab使用tip
### 1. 基本使用方法
- 基本使用方法与校内集群9.99使用方法类似，参见 https://git.ustc.edu.cn/ypb/gpu-cluster.

- 本集群内网IP：210.45.70.55

### 2. 用户自建镜像
本集群使用Dockerfile自建镜像。用户自建镜像分为两类，一类是以集群内部镜像为基础镜像构建，另一类是以外部网络镜像为基础镜像构建。
在Dockerfile中，FROM语句之后需要添加
```
ARG http_proxy=http://202.38.69.70:3129
ARG https_proxy=http://202.38.69.70:3129
```
来保证apt-get和pip install等命令可以正常接入网络。

用户在build后需要通知管理员，并提供**基础镜像名称、构建的镜像名称和构建镜像的目录**。用户构建的镜像名称以用户名开头，如`byf_py38_torch112`。

构建镜像需要在G121节点上，用户目录下新建一个临时文件夹，如`/ghome/byf/docker_tmp`，并将Dockerfile放在该文件夹中进行build。
build完成后通知管理员，由管理员负责推送至`bit:5000`总结点。之后管理员会删除相应的临时镜像。

目前服务器上存有的`bit:5000/deepo`镜像，已经可以满足绝大多数深度学习任务。

<span style="color:red">严禁！！！</span>
- 严禁build镜像后，自行push。

### 3. 终止任务
未正常终止的任务会导致GPU显存无法正常释放，在使用qdel删除job后，需要使用chk_gpuused命令查看资源是否正常释放，若没有正常释放，需要联系管理员使用clean_node G<node_id>进行释放，以防止其他任务因显存不够而中断。


### 已有镜像
| 拥有者 | 镜像名 |
|---------|---------|
| byf   | bit:5000/py38_vc    |
| yry   | bit:5000/yry_py3.10_pytorch2.1.2_cu12.1_devel   |
|tcb|bit:5000/tcb_pynew|

