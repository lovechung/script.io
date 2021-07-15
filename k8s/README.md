## 单机下kind搭建k8s集群开发环境（以CentOS为例）

### 安装kubectl
#### 使用yum安装
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

#### kubectl使用bash自动补全
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

### 安装kind
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
```
```bash
chmod +x ./kind
```
```bash
mv ./kind /usr/local/bin/
```
