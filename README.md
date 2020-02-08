# bitwarden-v2ray-wordpress-all-in-docker
此项目用docker部署bitwarden密码管理服务、wordpress内容管理平台、v2ray+ws+tls+cdn。

## 部署 
### Step 1 - 克隆版本库
在根目录下运行：
<pre>
$git clone https://github.com/TUT123456/bitwarden-v2ray-wordpress-all-in-docker.git
$mv bitwarden-v2ray-wordpress-all-in-docker docker
$cd docker
</pre>
### Step 2 - 修改配置
修改nginx/conf下配置文件内的域名和证书路径，
填完.env内的环境变量。
### Step 3 - 申请证书
这里我用的是certbot/dns-cloudflare，请酌情修改。
先去[cloudflare](https://dash.cloudflare.com/login)注册账号，并把域名交给cloudflare解析。并获取cloudflare api。
创建一个验证文件：
<pre>
$mkdir -p /root/docker
$touch /root/docker/cloudflare.ini
$vi /root/docker/cloudflare.ini
</pre>
在里面写入：
<pre>
# Cloudflare API credentials used by Certbot
dns_cloudflare_email = example@example.com
dns_cloudflare_api_key = e89af204ab0e06def9c0846c202d1dec40e80
</pre>
设置正确的权限：
<pre>
$chmod 400 /root/docker/cloudflare.ini
</pre>
之后执行一次性docker容器。
<pre>
$docker run -it --rm --name certbot \
            -v "/root/docker/nginx/certs:/etc/letsencrypt" \
            -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
            -v "/root/docker:/.secrets" \
            certbot/dns-cloudflare certonly \
            --dns-cloudflare-credentials /.secrets/cloudflare.ini \
            --dns-cloudflare-propagation-seconds 60 \
            --server https://acme-v02.api.letsencrypt.org/directory \
            -d '*.your.domain'
</pre>
运行过程中会让你输入邮箱。完成只会证书就会在/root/docker/nginx/certs/live/yourdomain/下。
如果需要延长证书，你只需要运行：
<pre>
$docker run -it --rm --name certbot -v "/root/docker/nginx/certs:/etc/letsencrypt" -v "/var/lib/letsencrypt:/var/lib/letsencrypt" -v "/root/docker/:/.secrets" certbot/dns-cloudflare renew
</pre>
即可，你可以加入crontab计划自动续期证书。

### Step 4 - 用docker-compose部署应用
<pre>$docker-compose up -d</pre>
### Step 5 - 配置v2ray
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
