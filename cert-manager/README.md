# cert-manager

管理K8s集群内cert-manager的Manifests.

## 特别说明

由于**cert-manager**是跨Namespace工作的组件，部分Role/RoleBinding资源需要特殊的Namespace，因此我们选择在ArgoCD创建Application时自动创建Namespace的方式进行管理，这种方式可以保证特殊Role/RoleBinding资源的Namespace不被修改.
