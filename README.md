# K8s-ImagePipeline
![image](https://github.com/user-attachments/assets/5b882a2d-98f0-42cd-9f34-e9b8f5cb6ff4)

# harbor 安裝
## Docker proxy配置
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
## 建置docker daemon.json
### vim /etc/docker/daemon.json
```
{
"insecure-registries": ["https://${registry_ip}"]
}
```
## harbor 下載
```
wget https://github.com/goharbor/harbor/releases/download/v2.12.2-rc1/harbor-offline-installer-v2.12.2-rc1.tgz
tar zxvf harbor-offline-installer-v2.12.2-rc1.tgz
cd harbor/
mv harbor.yml.tmpl harbor.yml

```
## 創建腳本-自簽憑證.sh
### vim ca.sh
```
#!/bin/bash
registry_ip="${ip}"
# 創建資料夾
mkdir -p /var/lib/harbor/data/cert/
# 產生自簽憑證
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -keyout harbor.key -out harbor.crt \
  -subj "/C=XX/L=Default City/O=Default Company Ltd/CN=${registry_ip}" \
  -addext "subjectAltName=IP:${registry_ip}"

# crt/key複製到registry底下
cp harbor.crt /var/lib/harbor/data/cert/
cp harbor.key /var/lib/harbor/data/cert/


# crt/key複製到docker底下
cp harbor.crt /etc/docker/certs.d/${registry_ip}
cp harbor.key /etc/docker/certs.d/${registry_ip}
systemctl daemon-reload
systemctl restart docker

```
## 修改 harbor.yml 
### 如圖片修改
![image](https://github.com/user-attachments/assets/90925320-f4b8-4599-a8cd-bfd39abb12d8)

```
hostname: ${ip}
certificate: /var/lib/harbor/data/cert/harbor.crt
private_key: /var/lib/harbor/data/cert/harbor.key

```
## docker login
```
docker login https://${registry_ip}
```
![image](https://github.com/user-attachments/assets/99129f1b-8237-43d7-8974-2f00f2029209)

## 登入harbor Web
### 建置專案
![image](https://github.com/user-attachments/assets/71d8a85e-06ee-473e-83e7-2ae9c0361fe0)
![image](https://github.com/user-attachments/assets/99da4072-c9d7-4ba4-9607-c879de74a220)

## 
