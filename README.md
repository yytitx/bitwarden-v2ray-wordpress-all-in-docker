# bitwarden-v2ray-wordpress-all-in-docker
docker快速搭建bitwarden+v2ray+wordpress服务

## 部署 
### Step 1
<pre>$git clone https://github.com/TUT123456/bitwarden-v2ray-wordpress-all-in-docker.git</pre>
### Step 2 - 修改conf文件夹下配置文件里的域名
### Step 3 - 用docker-compose部署应用
<pre>docker-compose up -d</pre>
### Step 4 - 申请证书
<pre>$docker exec -it nginx certbot-auto --nginx</pre>
执行这个命令后会自动根据nginx配置文件里的域名申请证书，这个过程中会要求输入邮箱地址。
如果要延长证书，请输入：
<pre>$docker exec -it nginx certbot-auto renew</pre>
可以把这个命令加入crontab计划
### Step 5 - 配置v2ray
v2ray配置文件在v2ray文件夹下，你只需要把uuid填上即可，关于生成uuid，可以[点击这里](https://www.uuidgenerator.net/)生成uuid。
