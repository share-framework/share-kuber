# 安装docker 和 docker-compose

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

docker-compose -v  
```

设置docker

配置加速，并设置驱动 
cat << EOF > /etc/docker/daemon.json 
{
    "registry-mirrors": [
"https://dockerhub.azk8s.cn",
"https://hub-mirror.c.163.com"
]
}
EOF

# 下载harbor安装包

https://github.com/goharbor/harbor/releases/tag/v2.5.0

下载下来之后解压

tar -xvzf harbor-offline-installer-v2.5.0.tgz   -C /usr/local/

修改配置文件
cp harbor.yml.tmpl harbor.yml

vim harbor.yml  

设置hostname为一个域名或者ip地址

注释443端口的https，有证书的话配置证书也可以

解压出来的harbor 执行

sh /usr/local/harbor/install.sh


 docker-compose ps

 查看是否安装完成

 默认的管理员用户名和密码是 admin/Harbor12345

 在对应docker机器上配置

 vim /etc/docker/daemon.json

{

"insecure-registries": ["http://harbor.andot.org"]

}
如果不配置，则会提示



 response from daemon: Get https://192.168.75.12/v2/: dial tcp 192.168.75.12:443: connect: connection refused

配置之后 systemctl daemon-reload && systemctl restart docker

然后进行登录操作

docker login -u lucas -p Ad123456 http://harbor.andot.org


192.168.1.69 harbor.andot.org

kubernetes 配置

kubectl create secret docker-registry docker-harbor  \
--docker-server=harbor.andot.org \
--docker-username=lucas \
--docker-password=Ad123456 \
--docker-email=andotorg@outlook.com