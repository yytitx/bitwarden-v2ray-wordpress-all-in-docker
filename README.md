# bitwarden-v2ray-wordpress-all-in-docker
docker快速搭建bitwarden+v2ray+wordpress服务

### 部署 
克隆这个库，然后用您的域名申请ssl证书放到cert文件夹下，同时修改conf文件夹下的nginx配置文件，然后就能docker-compose up -d部署docker了，最后运行目录下的shell文件修复wordpress的redis和安装插件问题。
