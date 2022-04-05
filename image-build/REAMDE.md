# 1. 制作镜像

脚本参数解释：
- rocketmq 版本号: 4.9.1
- m1-macos: 如果是 intel 或者 amd 芯片 centos 都可以
    
    sh build-image.sh 4.9.1 m1-macos

# 2. 关于 azul-jdk

后续 apache rocketmq 如果有重大升级,可更换 jdk.

Dockerfile-centos

```
RUN yum install -y java-1.8.0-openjdk-devel.x86_64 unzip gettext nmap-ncat openssl, which gnupg, telnet \
 && yum clean all -y
```

Dockerfile-macos

注意 jdk.zip 解压后文件层级为 jdk/jdk ，如果下载的 jdk 压缩包解压后的文件不是 jdk ,特别注意该 jdk 为 Oracle arrch64 jdk;

```
# 安装 zip 插件
RUN yum install -y unzip && yum clean all -y
# 添加 jdk jar 包
ADD jdk.zip /usr/local
# 移动 jdk jar 包到自定位置,jak 解压缩后的包在 jdk 中，此版本只适用于 m1 芯片
RUN  cd /usr/local && unzip jdk.zip -d jdk && mv ./jdk/jdk java;

# 配置环境变量
ENV JAVA_HOME /usr/local/java
ENV JRE_HOME /usr/local/java/jre
ENV PATH $JAVA_HOME/bin:$PATH
```

# oracle arrch 64 jdk.zip 包

下载好后，放在当前目录即可

链接: https://pan.baidu.com/s/1p1evXu5DmV03MuL8fSqPew 提取码: dh88

# centos 下载过慢，建议使用 alpine

# windows 目录下制作镜像

        sed -i 's/\r//' rocketmq/bin/runserver.sh;\
        sed -i 's/\r//' rocketmq/bin/runbroker.sh;