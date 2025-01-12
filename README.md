# Docker Images Pusher

使用Github Action将国外的Docker镜像转存到阿里云私有仓库，供国内服务器使用，免费易用<br>
- 支持DockerHub, gcr.io, k8s.io, ghcr.io等任意仓库<br>
- 支持最大40GB的大型镜像<br>
- 使用阿里云的官方线路，速度快<br>

视频教程：https://www.bilibili.com/video/BV1Zn4y19743/

作者：**[技术爬爬虾](https://github.com/tech-shrimp/me)**<br>
B站，抖音，Youtube全网同名，转载请注明作者<br>

## 使用方式


### 配置阿里云
登录阿里云容器镜像服务<br>
https://cr.console.aliyun.com/<br>
启用个人实例，创建一个命名空间（**ALIYUN_NAME_SPACE**）
![](/doc/命名空间.png)

访问凭证–>获取环境变量<br>
用户名（**ALIYUN_REGISTRY_USER**)<br>
密码（**ALIYUN_REGISTRY_PASSWORD**)<br>
仓库地址（**ALIYUN_REGISTRY**）<br>

![](/doc/用户名密码.png)


### Fork本项目
Fork本项目<br>
#### 启动Action
进入您自己的项目，点击Action，启用Github Action功能<br>
#### 配置环境变量
进入Settings->Secret and variables->Actions->New Repository secret
![](doc/配置环境变量.png)
将上一步的**四个值**<br>
ALIYUN_NAME_SPACE,ALIYUN_REGISTRY_USER，ALIYUN_REGISTRY_PASSWORD，ALIYUN_REGISTRY<br>
配置成环境变量

### 添加镜像
打开images.txt文件，添加你想要的镜像 
可以加tag，也可以不用(默认latest)<br>
可添加 --platform=xxxxx 的参数指定镜像架构<br>
可使用 k8s.gcr.io/kube-state-metrics/kube-state-metrics 格式指定私库<br>
可使用 #开头作为注释<br>
![](doc/images.png)
文件提交后，自动进入Github Action构建

### 使用镜像
回到阿里云，镜像仓库，点击任意镜像，可查看镜像状态。(可以改成公开，拉取镜像免登录)
![](doc/开始使用.png)

在国内服务器pull镜像, 例如：<br>
```
docker pull registry.cn-hangzhou.aliyuncs.com/shrimp-images/alpine
```
registry.cn-hangzhou.aliyuncs.com 即 ALIYUN_REGISTRY(阿里云仓库地址)<br>
shrimp-images 即 ALIYUN_NAME_SPACE(阿里云命名空间)<br>
alpine 即 阿里云中显示的镜像名<br>

### 多架构
需要在images.txt中用 --platform=xxxxx手动指定镜像架构
指定后的架构会以前缀的形式放在镜像名字前面
![](doc/多架构.png)

### 镜像重名
程序自动判断是否存在名称相同, 但是属于不同命名空间的情况。
如果存在，会把命名空间作为前缀加在镜像名称前。
例如:
```
xhofe/alist
xiaoyaliu/alist
```
![](doc/镜像重名.png)

### 定时执行
修改/.github/workflows/docker.yaml文件
添加 schedule即可定时执行(此处cron使用UTC时区)
![](doc/定时执行.png)


# 2. 其他Pull镜像的方法

### 方案一  转存到阿里云(本方法)
使用Github Action将国外的Docker镜像转存到阿里云私有仓库，供国内服务器使用，免费易用

- 支持DockerHub, gcr.io, k8s.io, ghcr.io等任意仓库
- 支持最大40GB的大型镜像
- 使用阿里云的官方线路，速度快

项目地址: 
https://github.com/tech-shrimp/docker_image_pusher

### 方案二 镜像站
现在只有很少的国内镜像站存活<br>
不保证镜像齐全,且用且珍惜<br>
以下三个镜像站背靠较大的开源项目，优先推荐<br>

|项目名称|项目地址| 加速地址|
| ----------- | ----------- |----------- |
|1Panel|[https://github.com/1Panel-dev/1Panel/](https://github.com/1Panel-dev/1Panel/)|https://docker.1panel.live|
|Daocloud|[https://github.com/DaoCloud/public-image-mirror](https://github.com/DaoCloud/public-image-mirror)|https://docker.m.daocloud.io|
|耗子面板|[https://github.com/TheTNB/panel](https://github.com/TheTNB/panel 	)|https://hub.rat.dev|


#### Linux配置镜像站




```
sudo vi /etc/docker/daemon.json
```
输入下列内容，最后按ESC，输入 :wq! 保存退出。
```
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://docker.1panel.live",
        "https://hub.rat.dev"
    ]
}
```
重启docker
```
sudo service docker restart
```

#### Windows/Mac配置镜像站
Setting->Docker Engine->添加上换源的那一段，如下图
![](/doc/win加速.png)

### 方案三 离线镜像
使用Github Action下载docker离线镜像
https://github.com/wukongdaily/DockerTarBuilder

### 方案四 使用一键脚本
bash -c "$(curl -sSLf https://xy.ggbond.org/xy/docker_pull.sh)" -s 完整镜像名

### 方案五 使用Cloudflare worker 自建镜像加速
https://github.com/cmliu/CF-Workers-docker.io

# 3. 去哪里找镜像

https://docker.fxxk.dedyn.io/

