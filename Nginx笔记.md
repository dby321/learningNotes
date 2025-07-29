# Nginx笔记

## Nginx简介

Nginx 是由俄罗斯开发者Igor Sysoev创建的，主要作为一个高性能的HTTP和反向代理服务器。它还支持作为SMTP、POP3和IMAP协议的代理服务器，有报告称，Nginx在能处理50000并发连接数。国内百度、新浪、腾讯等公司都使用Nginx作为WEB服务器。

- **官方中文文档**: [Nginx中文文档](https://blog.redis.com.cn/doc/index.html)
- **配置生成工具**: [DigitalOcean-Nginx配置在线生成](https://www.digitalocean.com/community/tools/nginx?global.app.lang=zhCN)
- [Linux搭建nginx+keepalived 高可用(主备+双主模式)](https://blog.csdn.net/weixin_52270081/article/details/118341576)

## Nginx常见概念

### 正向代理

正向代理（Proxy Server）是介于客户端与服务器之间的一个中间代理服务器，它用于拦截客户端的请求，并把请求转发给服务器，然后再把服务器的响应返回给客户端，VPN就是正向代理的一种。

### 反向代理

客户端对反向代理是无感知的，我们只需要把请求发给反向代理服务器，暴露的是代理服务器的地址，隐藏了服务器真实地址。

### 负载均衡

负载均衡（Load Balancing）是计算机网络技术，它将请求分发到多个服务器上，从而提高服务器的处理能力。

### 动静分离

动静分离（Static/Dynamic Separation）是网站部署的一种方式，它将静态资源（如HTML、CSS、JavaScript、图片等）和动态资源（如PHP、ASP、JSP等）分离，从而提高网站性能。

## Nginx安装

### 在线安装Nginx

Nginx 是一款高性能的 Web 服务器，广泛用于托管网站和应用程序。本文将介绍如何在 Linux 系统上安装、配置和启动 Nginx，并将其设置为系统服务。

#### 安装依赖项

在安装 Nginx 之前，需要先安装一些依赖项。打开终端并执行以下命令：

```bash
yum install -y wget gcc-c++ pcre-devel zlib-devel openssl-devel
```

这将安装必要的工具和库，以支持 Nginx 的编译和运行。

#### 下载 Nginx

从 Nginx 官网下载最新的稳定版本。例如，下载 Nginx 1.24.0 版本：

```bash
wget https://nginx.org/download/nginx-1.24.0.tar.gz
```

#### 解压 Nginx

解压下载的 Nginx 源代码包：

```bash
tar -zxvf nginx-1.24.0.tar.gz
```

#### 编译和安装

进入解压后的 Nginx 目录并进行编译和安装：

```bash
cd nginx-1.24.0
./configure
make && make install
```

Nginx 安装完成后，默认会创建 `/usr/local/nginx` 目录，并包含配置文件、日志文件等。

#### 配置防火墙

如果系统启用了防火墙，需要关闭防火墙：

```bash
systemctl stop firewalld
systemctl disable firewalld
```

#### 启动 Nginx

进入 Nginx 的安装目录并启动 Nginx 服务器：

```bash
cd /usr/local/nginx/sbin
./nginx
```

现在可以通过浏览器访问服务器的 IP 地址或域名来验证 Nginx 是否正常工作。

#### 配置 Nginx 为系统服务

将 Nginx 配置为系统服务，可以在系统启动时自动启动 Nginx。

1. 创建服务文件

在 `/etc/systemd/system/` 目录下创建一个新的服务文件，例如 `nginx.service`：

```bash
vi /etc/systemd/system/nginx.service
```

添加以下内容：

```ini
[Unit]
Description=Nginx HTTP Server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

2. 重新加载 systemd 配置文件

```bash
systemctl daemon-reload
```

3. 启动 Nginx 服务

```bash
systemctl start nginx
```

现在，Nginx 将作为系统服务在后台运行。

4. 设置开机自启动

```bash
systemctl enable nginx
```

这样，Nginx 将在系统启动时自动启动。

5. 检查和管理 Nginx 服务

- 检查 Nginx 状态：

```bash
systemctl status nginx
```

- 停止 Nginx 服务：

```bash
systemctl stop nginx
```

- 重启 Nginx 服务：

```bash
systemctl restart nginx
```

通过以上步骤，您可以在 Linux 系统上成功安装、配置和启动 Nginx，并将其设置为系统服务，以便在系统启动时自动启动 Nginx。这将为您的 Web 应用程序提供稳定性和高性能，同时为您提供了丰富的自定义选项，以满足各种需求。

### 使用Docker安装Nginx

Docker 是一个强大的容器化工具，而 Nginx 是一个高性能的 Web 服务器和反向代理。将两者结合可以为 Web 服务提供强大且可扩展的解决方案。以下是使用 Docker 部署 Nginx 的分步指南。

1. 拉取 Nginx 镜像

首先，你需要从 Docker Hub 拉取 Nginx 镜像。可以使用以下命令：

```bash
docker pull nginx
```

该命令将下载最新版本的 Nginx 镜像。如果需要特定版本，可以这样指定：

```bash
docker pull nginx:1.24.0
```

2. 创建挂载目录

在运行 Nginx 容器之前，需要在主机上创建目录来挂载配置文件、日志和网页内容。这可以确保即使容器被删除，数据也能保留。

```bash
mkdir -p /home/nginx/conf
mkdir -p /home/nginx/log
mkdir -p /home/nginx/html
```

3. 运行 Nginx 容器

现在可以运行 Nginx 容器并挂载之前创建的目录。该命令还会将容器的 80 端口映射到主机的 9002 端口。

```bash
docker run -p 9002:80 --name nginx \
-v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /home/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /home/nginx/log:/var/log/nginx \
-v /home/nginx/html:/usr/share/nginx/html \
-d nginx:latest
```

命令解析：

- `-p 9002:80`：将主机的 9002 端口映射到容器的 80 端口
- `--name nginx`：将容器命名为 "nginx"
- `-v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf`：挂载 Nginx 配置文件
- `-v /home/nginx/conf/conf.d:/etc/nginx/conf.d`：挂载额外的配置目录
- `-v /home/nginx/log:/var/log/nginx`：挂载日志目录
- `-v /home/nginx/html:/usr/share/nginx/html`：挂载网页内容目录
- `-d`：以分离模式运行容器

4. 复制配置文件

如果需要将默认配置文件从容器复制到主机，可以使用以下命令：

```bash
docker cp nginx:/etc/nginx/nginx.conf /home/nginx/conf/nginx.conf
docker cp nginx:/etc/nginx/conf.d /home/nginx/conf/conf.d
docker cp nginx:/usr/share/nginx/html /home/nginx/html
```
5. 重启容器

对配置文件进行任何更改后，需要重启 Nginx 容器以应用更改：

```bash
docker restart nginx
```

6. 测试设置

要验证 Nginx 是否正常运行，打开浏览器并访问 `http://localhost:9002`。你应该会看到 Nginx 的欢迎页面。

7. 总结

使用 Docker 部署 Nginx 简化了 Web 服务器的设置过程，并确保你的配置在不同环境中具有可移植性和一致性。通过以上步骤，你可以快速使用 Docker 启动并运行 Nginx。

## Nginx常见命令

- **启动Nginx**：`nginx`
- **停止Nginx**：`nginx -s stop`
- **重新加载Nginx**：`nginx -s reload`
- **重启Nginx**：`nginx -s restart`
- **查看Nginx状态**：`nginx -t`
- **查看Nginx版本**：`nginx -v`

## Nginx配置文件概述

Nginx的配置文件通常位于`/etc/nginx/nginx.conf`，可以通过`nginx -t`命令来检查配置文件的语法是否正确。

- 全局块用于设置Nginx的全局参数，如日志格式、错误日志级别、进程数等。
- 事件块用于设置Nginx的事件处理参数，如连接超时时间、最大连接数等。
- http块用于设置Nginx的HTTP相关参数，如服务器地址、端口号、文档根目录、错误页面等。

```nginx
worker_processes  1;  # 设置进程数
events {}  # 事件块，基本不需要修改
http {}  # http块，主要修改位置
```
## Nginx常见命令

- **启动Nginx**：`nginx`
- **停止Nginx**：`nginx -s stop`
- **重新加载Nginx**：`nginx -s reload`
- **重启Nginx**：`nginx -s restart`
- **查看Nginx状态**：`nginx -t`
- **查看Nginx版本**：`nginx -v`
## Tomcat安装

Tomcat是一个开放源代码的Java Servlet容器，用于在服务器上运行Java Web应用程序。以下是在Linux系统上安装Tomcat的分步指南：
1. 下载Tomcat：
 - 从Tomcat官网下载最新的Tomcat安装包。
 - 解压安装包到指定目录，例如`/opt/tomcat`。
2. 设置环境变量：
 - 编辑`/etc/profile`文件，在文件末尾添加以下内容：
  ```bash
   export CATALINA_HOME=/opt/tomcat
   export PATH=$PATH:$CATALINA_HOME/bin
  ```
3. 应用环境变量`source /etc/profile`
4. 启动Tomcat：
 - `cd $CATALINA_HOME`
 - `./bin/startup.sh`
 - 访问`http://localhost:8080`，如果看到Tomcat的欢迎页面，则表示安装成功。
5. 停止Tomcat：
 - `./bin/shutdown.sh`
6. 卸载Tomcat：
 - `rm -rf $CATALINA_HOME`

## 防火墙开放端口

防火墙的端口需要根据实际情况进行开放。
- **防火墙永久开放8080端口**：`firewall-cmd --add-port=8080/tcp --permanent`
- **重新加载防火墙配置**：`firewall-cmd --reload`
- **查看已经开放的端口**：`firewall-cmd --list-all`
## 反向代理配置

```
server {
	    listen server_port;# 换为具体服务器端口
	    server_name server_ip_address;# 换为具体服务器IP地址
	    location / {
	        proxy_pass http://myserver:port;
	    }
}
```

```
server {
  listen       server_port;
  server_name        server_ip_address；# 换为具体服务器IP地址
  location ~/location1/ {
      proxy_pass http://myserver1:port;
  }
  location ~/location2/ {
      proxy_pass http://myserver2:port;
  }
}
```
- `~`:用于表示uri包含正则表达式，并且区分大小写
- `~*`:用于表示uri包含正则表达式，并且不区分大小写
- `^~`:用于表示uri不包含正则表达式，并且区分大小写
- `=`:表示uri等于正则表达式，并且区分大小写

## 负载均衡配置

Nginx 负载均衡是一种高效的网络流量分配方式，它可以将客户端的请求分散到多个服务器上，从而提高整个应用的吞吐率和容错能力。在配置 Nginx 进行负载均衡时，主要涉及到 `proxy_pass` 指令和 `upstream` 模块。通过这些配置，Nginx 可以将请求代理转发到一组服务器，即虚拟服务池。

### 配置示例

以下是一个基本的 Nginx 负载均衡配置示例：

```nginx
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

### 配置说明

- **`upstream` 模块**：定义了一个名为 `myapp1` 的服务器组，包含三个服务器：`srv1.example.com`、`srv2.example.com` 和 `srv3.example.com`。
- **`server` 块**：监听 80 端口，并将根路径 (`/`) 的请求代理到 `myapp1` 服务器组。

### 负载均衡策略

Nginx 支持多种负载均衡策略，包括但不限于以下几种：

1. **轮询（Round Robin）**  
   这是默认的负载均衡方法，请求会按顺序分配到每个服务器。

2. **权重（Weighted Round Robin）**  
   可以为每个服务器设置权重，权重高的服务器会接收更多的请求。例如：
   ```nginx
   upstream myapp1 {
       server srv1.example.com weight=3;
       server srv2.example.com weight=2;
       server srv3.example.com weight=1;
   }
   ```

3. **IP 哈希（IP Hash）**  
   根据客户端的 IP 地址来分配请求，这样来自同一 IP 地址的请求总是发给同一个服务器，有助于解决会话保持的问题。例如：
   ```nginx
   upstream myapp1 {
       ip_hash;
       server srv1.example.com;
       server srv2.example.com;
       server srv3.example.com;
   }
   ```

4. **最少连接数（Least Connections）**  
   新的请求会发送到连接数最少的服务器。例如：
   ```nginx
   upstream myapp1 {
       least_conn;
       server srv1.example.com;
       server srv2.example.com;
       server srv3.example.com;
   }
   ```
## 动静分离配置

以下是优化后的 Markdown 格式内容，可直接复制到笔记中：

### 基础配置文件 (`nginx.conf`)
```nginx
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        server_name localhost;

        # 处理动态请求（转发到后端应用服务器）
        location / {
            proxy_pass http://localhost:81;
            proxy_set_header X-Real-IP $remote_addr;  # 透传客户端真实 IP
        }

        # 处理静态资源请求
        location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|js|css)$ {
            root static;      # 静态资源存放目录（相对路径为 Nginx 根目录下的 static/）
            expires 30d;      # 客户端缓存过期时间（30天）
        }
    }
}
```


### 配置说明

1. **动态请求处理**  
   - `location /`：匹配所有动态请求，通过 `proxy_pass` 转发到后端服务器（示例为 `localhost:81`）。
   - `proxy_set_header`：透传客户端真实 IP 到后端服务器，便于日志记录或业务处理。

2. **静态资源处理**  
   - `location ~ .*\.(扩展名)$`：通过正则匹配静态资源（如 `.html`, `.jpg`, `.js` 等）。
   - `root static`：指定静态资源根目录为 `static`（需在 Nginx 根目录下创建此目录）。
   - `expires 30d`：设置浏览器缓存时间为 30 天，减少重复请求。

---

### 操作步骤

1. **修改配置文件**  
   编辑 `nginx.conf` 文件，添加上述配置。

2. **创建静态资源目录**  
   ```bash
   mkdir -p /path/to/nginx/static   # 根据实际路径创建 static 目录
   ```
   将静态文件（如 HTML、图片、CSS、JS）放入此目录。

3. **重启 Nginx**  
   ```bash
   nginx -s reload   # 平滑重启（不中断服务）
   ```

### 效果验证

- **静态资源请求**  
  访问 `http://localhost/example.jpg`，Nginx 直接返回 `static/example.jpg`，响应头包含 `Cache-Control: max-age=2592000`（30 天缓存）。

- **动态请求**  
  访问 `http://localhost/api`，请求被转发至 `localhost:81` 处理，后端服务器日志中会记录客户端真实 IP。

## 高可用集群配置-主从节点

以下是 Nginx 高可用集群的配置示例，包括主从节点和热备节点。
### keepAlived 安装
1. 安装基础依赖包

确保系统已安装以下依赖包。未安装的话，请执行：

```bash
yum install -y gcc openssl-devel libnl libnl-devel libnfnetlink-devel net-tools vim
```

2. 下载并解压最新版 Keepalived

下载官网提供的最新版（目前为2.2.2）：

```bash
wget https://www.keepalived.org/software/keepalived-2.2.2.tar.gz
```

解压缩并将文件夹重命名为 `keepalived` 移至 `/usr/local/` 目录下：

```bash
tar -zxvf keepalived-2.2.2.tar.gz
mv keepalived-2.2.2 /usr/local/keepalived
```

3. 编译并安装

进入目录进行配置和编译安装：

```bash
cd /usr/local/keepalived/
./configure
make && make install
```

4. 复制相关文件到系统中

根据您的文件路径，将必要的文件复制到系统对应的目录：

```bash
mkdir /etc/keepalived
cp /usr/local/keepalived/keepalived/etc/init.d/keepalived /etc/init.d/
cp /usr/local/keepalived/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/
cp /usr/local/keepalived/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
cp /usr/local/sbin/keepalived /usr/sbin/
```
5. 启动测试

完成上述步骤后，启动 Keepalived 测试是否安装成功：

```bash
/etc/init.d/keepalived start
```
### keepAlived配置

1. 备份原配置文件

在修改 `/etc/keepalived/keepalived.conf` 之前，建议先备份原配置文件：

```bash
cp /etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf.bak
```

然后使用 `vim` 或其他文本编辑器打开并编辑该配置文件：

```bash
vim /etc/keepalived/keepalived.conf
```

2. 基础模块说明

Keepalived 配置主要涉及三个基础模块：`global_defs`、`vrrp_instance` 和 `vrrp_script`。

3.  主服务器（172.16.2.50）配置示例

```conf
global_defs {               
   router_id Nginx_01
}
vrrp_script check_nginx {
    script "/etc/keepalived/check_nginx.sh"
    interval 2
    weight -5
    fall 3
    rise 2
}
vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.200
    }
    track_script {
        check_nginx
    }
}
```

4. 备用服务器（172.16.2.51）配置示例

与主服务器配置基本相同，但有以下几点不同：

- `router_id` 不同。
- `state` 设置为 `BACKUP`。
- `priority` 较低，通常设置为 100。
- `virtual_router_id` 必须相同。

```conf
global_defs {               
   router_id Nginx_02
}
vrrp_script check_nginx {
    script "/etc/keepalived/check_nginx.sh"
    interval 2
    weight -5
    fall 3
    rise 2
}
vrrp_instance VI_1 {
    state BACKUP
    interface ens33
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.200
    }
    track_script {
        check_nginx
    }
}
```

注意：
- 确保两个节点的 `virtual_router_id` 相同。
- `check_nginx.sh` 脚本应提前编写好，并放置于指定路径下。

### Nginx 和 Keepalived 简易监控脚本
> TODO 以后研究一下这两个脚本的实现原理
1. 基础版本：仅停止 Keepalived 服务

此脚本用于检测 Nginx 进程是否存在，如果不存在，则停止 Keepalived 服务。

```bash
#!/bin/bash
pidof nginx
if [ $? -ne 0 ];then
    /etc/init.d/keepalived stop
fi
```

2. 高级版本：尝试重启 Nginx 并在失败后停止 Keepalived

此脚本首先尝试启动 Nginx。如果两次尝试后 Nginx 仍未运行，则停止 Keepalived 服务。

```bash
#!/bin/bash
counter=$(ps -C nginx --no-heading|wc -l)
if [ "${counter}" = "0" ]; then
    /usr/local/bin/nginx
    sleep 2
    counter=$(ps -C nginx --no-heading|wc -l)
    if [ "${counter}" = "0" ]; then
        /etc/init.d/keepalived stop
    fi
fi
```

### Nginx 配置示例

只需将 `server_name` 更改为 VIP 的 IP 地址，即可实现通过访问该 VIP 地址进行负载均衡。

```nginx
upstream phpserver1 {
    server 172.16.2.20:9091;
    server 172.16.2.21:9092;
}

server {
    listen       80;
    server_name  192.168.200.200; # 将这里的 IP 改为你的 VIP 地址
    location / {
        proxy_pass   http://phpserver1;
        index  index.html index.htm;
    }
}
```
### keepalived启动与测试


1. **启动 Nginx 服务**

   在两台 Nginx 服务器上分别执行以下命令以重启 Nginx 服务：

   ```bash
   systemctl restart nginx.service
   ```

2. **启动 Keepalived 服务**

   确保 Nginx 服务已经成功重启后，在两台服务器上分别启动 Keepalived 服务：

   ```bash
   systemctl start keepalived
   ```
## 高可用集群配置-双主模式


1. Nginx1 配置文件

```conf
global_defs {               
   router_id Nginx_01
}
vrrp_script check_nginx {
    script "/etc/keepalived/check_nginx.sh"
    interval 2
    weight -5
    fall 3
    rise 2
}
vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.200
    }
    track_script {
        check_nginx
    }
}   
vrrp_instance VI_2 {
    state BACKUP
    interface ens33
    virtual_router_id 52
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.210
    }
    track_script {
        check_nginx
    }    
}
```

2. Nginx2 配置文件

```conf
global_defs {               
   router_id Nginx_02
}
vrrp_script check_nginx {
    script "/etc/keepalived/check_nginx.sh"
    interval 2
    weight -5
    fall 3
    rise 2
}
vrrp_instance VI_1 {
    state BACKUP
    interface ens33
    virtual_router_id 51
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.200
    }
    track_script {
        check_nginx
    }
}
vrrp_instance VI_2 {
    state MASTER
    interface ens33
    virtual_router_id 52
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.210
    }
    track_script {
        check_nginx
    }    
}
```

3. Nginx 负载均衡配置

在 Nginx 配置文件中添加第二个 VIP 地址 `192.168.200.210`：

```nginx
upstream phpserver1 {
    server 172.16.2.20:9091;
    server 172.16.2.21:9092;
}

server {
    listen       80;
    server_name  192.168.200.200;
    server_name  192.168.200.210;
    location / {
        proxy_pass   http://phpserver1;
        index  index.html index.htm;
    }     
}
4. 测试
4.1 **依次重启两台服务器的 Keepalived 服务**

   在每台服务器上执行以下命令以重启 Keepalived 服务：

   ```bash
   systemctl restart keepalived
```

4.2 **查看 IP 地址**

   使用以下命令检查虚拟 IP 是否已正确分配到相应的服务器：

   ```bash
   ip addr show
   ```

   **预期结果：** 应看到两个虚拟 IP 地址 `192.168.200.200` 和 `192.168.200.210` 分别被分配到不同的服务器。

   > 在这里插入图片描述

4.3 **访问 VIP 地址进行负载均衡测试**

   在浏览器中分别访问以下两个地址，确保它们都能正确地将请求分发到后端的 Web 服务器（web1 和 web2）：

   - [http://192.168.200.200](http://192.168.200.200)
   - [http://192.168.200.210](http://192.168.200.210)

## keepAlived配置参考

```
#全局配置
global_defs {
   # 邮件通知信息
   notification_email {
     # 定义收件人
     acassen@firewall.loc
   }
   # 定义发件人
   notification_email_from Alexandre.Cassen@firewall.loc
   # SMTP服务器地址
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   # 路由器标识，一般不用改，也可以写成每个主机自己的主机名
   router_id LVS_DEVEL
   # VRRP的ipv4和ipv6的广播地址，配置了VIP的网卡向这个地址广播来宣告自己的配置信息，下面是默认值
   vrrp_mcast_group4 224.0.0.18
   vrrp_mcast_group6 ff02::12
}

# 定义用于实例执行的脚本内容，比如可以在线降低优先级，用于强制切换
vrrp_script SCRIPT_NAME {

}

# 一个vrrp_instance就是定义一个虚拟路由器的，实例名称
vrrp_instance VI_1 {
    # 定义初始状态，可以是MASTER或者BACKUP
    state MASTER
    # 工作接口，通告选举使用哪个接口进行
    interface ens33
    # 虚拟路由ID，如果是一组虚拟路由就定义一个ID，如果是多组就要定义多个，而且这个虚拟
    # ID还是虚拟MAC最后一段地址的信息，取值范围0-255
    virtual_router_id 51
    # 使用哪个虚拟MAC地址
    use_vmac XX:XX:XX:XX:XX
    # 监控本机上的哪个网卡，网卡一旦故障则需要把VIP转移出去
    track_interface {
        eth0
        ens33
    }
    # 如果你上面定义了MASTER,这里的优先级就需要定义的比其他的高
    priority 100
    # 通告频率，单位为秒
    advert_int 1
    # 通信认证机制，这里是明文认证还有一种是加密认证
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    # 设置虚拟VIP地址，一般就设置一个，在LVS中这个就是为LVS主机设置VIP的，这样你就不用自己手动设置了
    virtual_ipaddress {
        # IP/掩码 dev 配置在哪个网卡
        192.168.200.16/24 dev eth1
        # IP/掩码 dev 配置在哪个网卡的哪个别名上
        192.168.200.17/24 dev label eth1:1
    }
    # 虚拟路由，在需要的情况下可以设置lvs主机 数据包在哪个网卡进来从哪个网卡出去
    virtual_routes {
        192.168.110.0/24 dev eth2
    }
    # 工作模式，nopreempt表示工作在非抢占模式，默认是抢占模式 preempt
    nopreempt|preempt
    # 如果是抢占默认则可以设置等多久再抢占，默认5分钟
    preempt delay 300
    # 追踪脚本，通常用于去执行上面的vrrp_script定义的脚本内容
    track_script {

    }
    # 三个指令，如果主机状态变成Master|Backup|Fault之后会去执行的通知脚本，脚本要自己写
    notify_master ""
    notify_backup ""
    notify_fault ""
}

# 定义LVS集群服务，可以是IP+PORT；也可以是fwmark 数字，也就是防火墙规则
# 所以通过这里就可以看出来keepalive天生就是为ipvs而设计的
virtual_server 10.10.10.2 1358 {
    delay_loop 6
    # 算法
    lb_algo rr|wrr|lc|wlc|lblc|sh|dh 
    # LVS的模式
    lb_kind NAT|DR|TUN
    # 子网掩码，这个掩码是VIP的掩码
    nat_mask 255.255.255.0
    # 持久连接超时时间
    persistence_timeout 50
    # 定义协议
    protocol TCP
    # 如果后端应用服务器都不可用，就会定向到那个服务器上
    sorry_server 192.168.200.200 1358

    # 后端应用服务器 IP PORT
    real_server 192.168.200.2 1358 {
        # 权重
        weight 1
        # MSIC_CHECK|SMTP_CHEKC|TCP_CHECK|SSL_GET|HTTP_GET这些都是
        # 针对应用服务器做健康检查的方法
        MISC_CHECK {}
        # 用于检查SMTP服务器的
        SMTP_CHEKC {}

        # 如果应用服务器不是WEB服务器，就用TCP_CHECK检查
        TCP_CHECK {
          # 向哪一个端口检查，如果不指定默认使用上面定义的端口
          connect_port <PORT>
          # 向哪一个IP检测，如果不指定默认使用上面定义的IP地址
          bindto <IP>
          # 连接超时时间
          connect_timeout 3
        }

        # 如果对方是HTTPS服务器就用SSL_GET方法去检查，里面配置的内容和HTTP_GET一样
        SSL_GET {}

        # 应用服务器UP或者DOWN，就执行那个脚本
        notify_up "这里写的是路径，如果脚本后有参数，整体路径+参数引起来"
        notify_down "/PATH/SCRIPTS.sh 参数"

        # 使用HTTP_GET方法去检查
        HTTP_GET {
            # 检测URL
            url { 
              # 具体检测哪一个URL
              path /testurl/test.jsp
              # 检测内容的哈希值
              digest 640205b7b0fc66c1ea91c463fac6334d
              # 除了检测哈希值还可以检测状态码，比如HTTP的200 表示正常，两种方法二选一即可
              status_code 200
            }
            url { 
              path /testurl2/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334d
            }
            url { 
              path /testurl3/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334d
            }
            # 向哪一个端口检查，如果不指定默认使用上面定义的端口
            connect_port <PORT>
            # 向哪一个IP检测，如果不指定默认使用上面定义的IP地址
            bindto <IP>
            # 连接超时时间
            connect_timeout 3
            # 尝试次数
            nb_get_retry 3
            # 每次尝试之间间隔几秒
            delay_before_retry 3
        }
    }

    real_server 192.168.200.3 1358 {
        weight 1
        HTTP_GET {
            url { 
              path /testurl/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334c
            }
            url { 
              path /testurl2/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334c
            }
            connect_timeout 3
            nb_get_retry 3
            delay_before_retry 3
        }
    }
}

```
## Nginx架构原理与优化

### 进程模型
- Master-Worker架构
  - Master进程：负责配置文件解析、worker进程管理（启动/终止/信号处理）
  - Worker进程：实际处理网络请求（默认数量=CPU核心数，通过worker_processes auto自动检测）
  - 热升级机制：通过新旧worker共存实现无缝升级

### 事件驱动模型
1. 多路复用I/O（epoll/kqueue）
   - 单线程异步非阻塞处理上万并发连接
   - 通过事件分发器监控socket状态变化
2. 请求处理流程：
   ┌─────────────┐       ┌──────────────┐
   │ 新连接到达   │───►│ 加入epoll监控 │
   └─────────────┘       └──────────────┘
           ▼
   ┌────────────────┐
   │ 数据可读/可写时  │
   │ 触发事件回调处理 │
   └────────────────┘

### 高可用设计
- Worker隔离性：单个worker崩溃不影响整体服务
- 零停机重载配置：
  ```nginx
  nginx -s reload  # 平滑加载新配置
  ```
- 连接限流机制：限制单个worker的最大连接数（worker_connections 1024）

### 性能优化建议
1. CPU亲和性配置（减少上下文切换）：
   ```nginx
   worker_cpu_affinity auto;
   ```
2. 文件描述符优化：
   ```nginx
   worker_rlimit_nofile 65535; # 突破系统限制
   ```
3. 事件处理优化：
   ```nginx
   use epoll; # Linux环境推荐
   multi_accept on; # 一次性接受所有新连接
   ```
4. 负载均衡策略：支持加权轮询/IP哈希等算法
