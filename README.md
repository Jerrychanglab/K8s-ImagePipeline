# K8s-ImagePipeline
# harbor 安裝
## harbor 下載
``wget https://github.com/goharbor/harbor/releases/download/v2.12.2-rc1/harbor-offline-installer-v2.12.2-rc1.tgz``
## docker proxy配置
### vim /etc/systemd/system/docker.service.d/http-proxy.conf
```
[Service]
Environment="HTTP_PROXY=http://${IP}:3128"
Environment="HTTPS_PROXY=http://${IP}:3128"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```
## VM Proxy配置
```
cat <<EOF >> ~/.bashrc
export http_proxy=http://10.33.55.87:3128
export https_proxy=http://10.33.55.87:3128
EOF
source ~/.bashrc
```
