---
title: springboot使用docker容器化
date: 2020-03-04 11:54:24
tags:
  - springboot
  - docker
---

### Dockerfile
在项目目录下/src/main 建立docker目录，建立Dockerfile文件
```bash
# 该镜像需要依赖的基础镜像
FROM java:8
ARG workdir=/app
# VOLUME 指定了临时文件目录为/tmp。
# 其效果是在主机 /var/lib/docker 目录下创建了一个临时文件，并链接到容器的workdir
VOLUME ${workdir}
# 指定工作目录，下面的指令操作将在这个指定目录中执行。当通过交互模式的exec命令进入到该容器时，默认当前路径是/app
WORKDIR ${workdir}
# 将jar包添加到容器中并更名
ADD demo.jar app.jar
# 声明服务运行在8888端口
EXPOSE 8888
# 指定docker容器启动时运行jar包
ENTRYPOINT ["sh","-c","java -Dconfig_user=$config_user -Dconfig_password=$config_password -Dplatform_type=$platform_type -Deureka_uri=$eureka_uri -Dspring.application.name=$spring.application.name -jar app.jar"]
# 指定维护者的名字
MAINTAINER donghao
```
Java启动脚本中加入shell变量，通过docker 动态参数传递到springboot容器内，用来改变启动的环境。

### 添加maven插件
```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.18.1</version>
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <!-- Docker maven plugin -->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.0.0</version>
                <configuration>
                    <imageName>springboot/${project.artifactId}</imageName>
                    <dockerDirectory>src/main/docker</dockerDirectory>
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>
                </configuration>
            </plugin>
            <!-- Docker maven plugin -->
        </plugins>
    </build>
```
### 构建镜像
```bash
mvn clean package -Dmaven.test.skip=true docker:build
```
执行成功后可以通过`docker images`查看镜像
### 通过docker容器启动springboot
```bash
docker run -e config_user=aaa -e config_password=aaa -e platform_type=dev -e eureka_uri=http://127.0.0.1:8761/eureka/ -e spring.application.name=demo-donghao -p 8888:8888 -v d:\aaa:/app /logs -t springboot/crm-admin
```
>-e 注入启动参数，会传递到springboot中，必须在-p之前
-p 端口映射
-v 文件挂载,将容器内的日志挂载到本地，方便日志查看
-d 后台运行容器，并返回容器ID
>
可通过`docker exec -it <容器name> /bin/bash`进入容器。
### 遇到的问题
- 本地 `mvn package docker:build` 连接不上docker
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/docker%20connect.png)
开放端口
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/%E5%BC%80%E6%94%BE2375%E7%AB%AF%E5%8F%A3.png)
- Dockerfile cannot be Empty
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/dockerfile%E4%B8%BA%E7%A9%BA.png)
先吧Dockerfile写完整之后再进行docker build 的过程。
***参考***
1.[https://my.oschina.net/thinwonton/blog/2967118](https://my.oschina.net/thinwonton/blog/2967118)