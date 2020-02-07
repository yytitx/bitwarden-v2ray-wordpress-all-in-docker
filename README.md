# bitwarden-v2ray-wordpress-all-in-docker
docker快速搭建bitwarden+v2ray+wordpress服务

## 部署 
### Step 1 - 克隆版本库
<pre>$git clone https://github.com/TUT123456/bitwarden-v2ray-wordpress-all-in-docker.git</pre>
### Step 2 - 修改配置
把conf文件夹下两个nginx配置文件内的域名都改成你自己的。
把.env内的参数也调成你自己的。
### Step 3 - 用docker-compose部署应用
<pre>docker-compose up -d</pre>
### Step 4 - 申请证书
<pre>$docker exec -it nginx certbot-auto --nginx</pre>
执行这个命令后会自动根据nginx配置文件里的域名申请证书，这个过程中会要求输入邮箱地址。
如果要延长证书，请输入：
<pre>$docker exec -it nginx certbot-auto renew</pre>
可以把这个命令加入crontab计划
### Step 5 - 修复wordpress安装插件要求填入ftp的问题和给wordpress添加redis缓存。
执行目录下的脚本：<pre>./install_redis.sh</pre>
然后在wordpress安装Redis Object Cache插件。
### Step 6 - 配置v2ray
v2ray配置文件在v2ray文件夹下，你只需要把uuid填上即可，关于生成uuid，可以[点击这里](https://www.uuidgenerator.net/)生成uuid。
请手动输入客户端配置，客户端配置如下：
<pre>
    地址: 你的域名
    端口：443
    用户id：生成的uuid
    额外id：64
    加密方式：none
    传输方式：ws
    伪装类型：none
    伪装域名：你的域名
    path：/scret
    底层传输安全：tls
    
</pre>
