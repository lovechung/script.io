# 单机下kind搭建k8s集群开发环境（以CentOS 8.4为例）
注：CentOS 8.4安装docker前，需要先卸载自带的podman，不然你要倒霉
```bash
yum -y erase podman buildah
```

## 安装kubectl
### 使用yum安装
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```
```bash
yum install -y kubectl
```

### kubectl使用bash自动补全
```bash
yum install bash-completion
```
```bash
echo 'source <(kubectl completion bash)' >>~/.bashrc
```
```bash
kubectl completion bash >/etc/bash_completion.d/kubectl
```
```bash
source ~/.bashrc
```

## 安装kind
### 下载kind
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
```
```bash
chmod +x ./kind
```
```bash
mv ./kind /usr/local/bin/
```

### 创建集群
```bash
kind create cluster --config=kind-config.yaml --image=kindest/node:v1.21.1
```

## 安装istio
### 安装istioctl
```bash
curl -sL https://istio.io/downloadIstioctl | sh -
```
```bash
mv ～/.istioctl/bin/istioctl /usr/local/bin/
```

## 部署bookinfo示例应用
### 创建命名空间
```bash
kubectl apply -f https://raw.githubusercontent.com/lovechung/script.io/main/k8s/istio/namespace.yaml
```
### 部署示例应用
```bash
kubectl apply -n bookinfo -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/platform/kube/bookinfo.yaml
```
### 监控pod部署情况
```bash
watch kubectl get pod -n bookinfo
```
### 创建网关
```bash
kubectl apply -n bookinfo -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/networking/bookinfo-gateway.yaml
```

## 安装istio的可视化UI
### 安装kiali
```bash
kubectl apply -n istio-system -f https://raw.githubusercontent.com/istio/istio/master/samples/addons/kiali.yaml
```

### 安装prometheus
```bash
kubectl apply -n istio-system -f https://raw.githubusercontent.com/istio/istio/master/samples/addons/prometheus.yaml
```

### 创建网关
```bash
kubectl apply -n istio-system -f https://raw.githubusercontent.com/lovechung/script.io/main/k8s/istio/kiali-gateway.yaml
```

### 获取kiali登录token
```bash
kubectl get secret -n istio-system $(kubectl get sa kiali -n istio-system -o jsonpath={.secrets[0].name}) -o jsonpath={.data.token} | base64 -d
```

## 安装kuboard
### docker-compose.yml
```yml
version: '3'
services:
  kuboard:
    container_name: kuboard
    image: eipwork/kuboard:v3
    restart: always
    ports:
      - 8880:80/tcp
      - 8881:10081/tcp
    environment:
      KUBOARD_ENDPOINT: 'http://192.168.0.223:8880'
      KUBOARD_AGENT_SERVER_TCP_PORT: '8881'
    volumes:
      - ~/data/kuboard:/data
```




