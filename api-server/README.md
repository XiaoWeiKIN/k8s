## K8S申明式API

`kubectl replace` 的执行过程，是使用新的YAML文件中的API对象，替换原有的API对象；而`kubectl apply`，则是执行了一个对原有API对象的PATCH（补丁）操作。

这意味着APIServer在响应命令式请求（比如`kubectl replace`)的时候，一次只能处理一个写请求，否则会有产生冲突的可能。而对于声明式请求（比如`kubectl apply`），一次能处理多个写操作，并且具备Merge能力。

## DAC（Dynamic Admission Control），Initializer

## CRD (Custom Resource Definition)

自定义API资源

## K8S多版本机制

Group：资源组
Version：资源版本
Resource：资源
Kind：资源类型

K8s支持多个Group，每个Group支持多个Version，每个Version支持多个Resource，其中部分资源同时会拥有自己的子资源（即SubResource）。例如，Deployment资源拥有Status子资源。

K8s的完整资源表示形式`group/version/resource/subresource`


## 创建资源对象过程

Kubernetes Pod资源对象创建流程介绍如下。
1. 使用kubectl工具向Kubernetes API Server发起创建Pod资源对象的请求。
   1.2 首先进入restful-go的MUX和Routes流程完成URL和对应资源Handler的绑定。
   1.3 然后根据Yaml中的描述创建资源对象，APIServer进行Convert工作将外部资源版本转换为内部资源版本SuperVersion。
   1.4 然后进行Admission()和Validation()操作。
   1.5 对资源对象进行序列化操作。
2. Kubernetes API Server验证请求并将其持久保存到Etcd集群中。
3. Kubernetes API Server基于Watch机制通知kube-scheduler调度器。
4. kube-scheduler调度器根据预选和优选调度算法为Pod资源对象选择最优的节点并通知Kubernetes API Server。
5. Kubernetes API Server将最优节点持久保存到Etcd集群中。
6. Kubernetes API Server通知最优节点上的kubelet组件。
7. kubelet组件在所在的节点上通过与容器进程交互创建容器。
8. kubelet组件将容器状态上报至Kubernetes API Server。
9. Kubernetes API Server将容器状态持久保存到Etcd集群中。

## Informer机制

