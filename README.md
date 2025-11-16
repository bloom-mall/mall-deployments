# Mall Deployments

管理K8s集群内基础设施、Mall相关应用/中间件的Manifests.

## 说明

- 一级目录映射ArgoCD里的Application, 保持同名
- 一级目录映射K8s集群里的Namespace, 保持同名

## 新k8s集群如何部署ArgoCD

### 1. 安装ArgoCD

#### 1.1 创建ArgoCD命名空间

```bash
kubectl create namespace argocd
```

#### 1.2 将 [deploy.yaml](argocd/base/deploy.yaml) 文件上传到 `k8s` 集群控制平面节点

```bash
scp -i <your_pem_file_path> ./argocd/base/deploy.yaml  ubuntu@your_ip:/home/ubuntu
```

#### 1.3 部署 ArgoCD 到 K8s集群

```bash
kubectl apply -n argocd -f deploy.yaml
```

#### 1.4 等待所有 ArgoCD 相关的 pod 启动成功

#### 1.5 下载 ArgoCD CLI 并上传到 `k8s` 集群控制平面节点

下载最新的二进制包，https://github.com/argoproj/argo-cd/releases

#### 1.5 安装 ArgoCD CLI

```bash
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
```

#### 1.6 将 `k8s` 集群内的 `ArgoCD` 服务临时暴露

```bash
kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443
```

#### 1.7 设置密码

```bash
# 记录打印出来的一次性密码
argocd admin initial-password -n argocd

# 使用用户名`admin`和刚才的一次性密码登录
argocd login 127.0.0.1:8080

# 设置新密码
argocd account update-password
```

### 2. 浏览器访问上一步中暴露的 `ArgoCD` Web端，创建 application 即可

***注意***： 需要临时设置防火墙允许8080端口

url: http://<控制平面节点公网IP>:8080

## Argocd 应用创建顺序

- cert-manager
- infrastructure
- argocd
- mall
