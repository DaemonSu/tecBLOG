# kubectl概述

kubectl是Kubernetes集群的命令行工具，通过kubectl能够对集群本身进行管理，并能够在集群上进行容器化应用的安装部署。运行kubectl命令的语法如下所示：

```html
$ kubectl [command] [TYPE] [NAME] [flags]
```

**comand**：指定要对资源执行的操作，例如create、get、describe和delete

**TYPE**：指定资源类型，资源类型是大小学敏感的，开发者能够以单数、复数和缩略的形式。例如：

```html
$ kubectl get pod pod1 



$ kubectl get pods pod1 



$ kubectl get po pod1
```

- **NAME**：指定资源的名称，名称也大小写敏感的。如果省略名称，则会显示所有的资源，例如:

```html
 $ kubectl get pods
```

- **flags**：指定可选的参数。例如，可以使用-s或者–server参数指定Kubernetes API server的地址和端口。

另外，可以通过kubectl help命令获取更多的信息。

```
Basic Commands (Beginner):

  create         Create a resource from a file or from stdin.

  expose         使用 replication controller, service, deployment 或者 pod 并暴露它作为一个新的Kubernetes Service

  run            在集群中运行一个指定的镜像

  set            为 objects 设置一个指定的特征

Basic Commands (Intermediate):

  explain        查看资源的文档

  get            显示一个或更多 resources

  edit           在服务器上编辑一个资源

  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector


Deploy Commands:

  rollout        Manage the rollout of a resource

  scale          为 Deployment, ReplicaSet, Replication Controller 或者 Job 设置一个新的副本数量

  autoscale      自动调整一个 Deployment, ReplicaSet, 或者 ReplicationController 的副本数量


Cluster Management Commands:

  certificate    修改 certificate 资源.


  cluster-info   显示集群信息

  top            Display Resource (CPU/Memory/Storage) usage.

  cordon         标记 node 为 unschedulable

  uncordon       标记 node 为 schedulable

  drain          Drain node in preparation for maintenance

  taint          更新一个或者多个 node 上的 taints

Troubleshooting and Debugging Commands:

  describe       显示一个指定 resource 或者 group 的 resources 详情

  logs           输出容器在 pod 中的日志

  attach         Attach 到一个运行中的 container

  exec           在一个 container 中执行一个命令

  port-forward   Forward one or more local ports to a pod

  proxy          运行一个 proxy 到 Kubernetes API server

  cp             复制 files 和 directories 到 containers 和从容器中复制 files 和 directories.

  auth           Inspect authorization

Advanced Commands:

  apply          通过文件名或标准输入流(stdin)对资源进行配置

  patch          使用 strategic merge patch 更新一个资源的 field(s)

  replace        通过 filename 或者 stdin替换一个资源

  wait           Experimental: Wait for one condition on one or many resources

  convert        在不同的 API versions 转换配置文件

Settings Commands:


  label          更新在这个资源上的 labels

  annotate       更新一个资源的注解

  completion     Output shell completion code for the specified shell (bash or zsh)

Other Commands:

  alpha          Commands for features in alpha

  api-resources  Print the supported API resources on the server

  api-versions   Print the supported API versions on the server, in the form of "group/version"

  config         修改 kubeconfig 文件

  plugin         Runs a command-line plugin

  version        输出 client 和 server 的版本信息
```

# kubectl的常用命令

kubectl作为kubernetes的命令行工具，主要的职责就是对集群中的资源的对象进行操作，这些操作包括对资源对象的创建、删除和查看等。下表中显示了kubectl支持的所有操作，以及这些操作的语法和描述信息：

| 操作           | 语法                                                         | 描述                                                        |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| annotate       | kubectl annotate (-f FILENAME \| TYPE NAME \| TYPE/NAME) KEY_1=VAL_1 …  KEY_N=VAL_N [–overwrite] [–all] [–resource-version=version] [flags] | 添加或更新一个或多个资源的注释                              |
| api-versions   | kubectl api-versions [flags]                                 | 列出可用的API版本                                           |
| apply          | kubectl apply -f FILENAME [flags]                            | 将来自于文件或stdin的配置变更应用到主要对象中。             |
| attach         | kubectl attach POD -c CONTAINER [-i] [-t] [flags]            | 连接到正在运行的容器上，以查看输出流或与容器交互（stdin）。 |
| autoscale      | kubectl autoscale (-f FILENAME \| TYPE NAME \| TYPE/NAME) [–min=MINPODS] –max=MAXPODS [–cpu-percent=CPU] [flags] | 自动扩宿容由副本控制器管理的Pod。                           |
| cluster-info   | kubectl cluster-info [flags]                                 | 显示群集中的主节点和服务的的端点信息。                      |
| config         | kubectl config SUBCOMMAND [flags]                            | 修改kubeconfig文件。                                        |
| create         | kubectl create -f FILENAME [flags]                           | 从文件或stdin中创建一个或多个资源对象。                     |
| delete         | kubectl delete (-f FILENAME \| TYPE [NAME \| /NAME \| -l label \| –all]) [flags] | 删除资源对象。                                              |
| describe       | kubectl describe (-f FILENAME \| TYPE [NAME_PREFIX \| /NAME \| -l label]) [flags] | 显示一个或者多个资源对象的详细信息。                        |
| edit           | kubectl edit (-f FILENAME \| TYPE NAME \| TYPE/NAME) [flags] | 通过默认编辑器编辑和更新服务器上的一个或多个资源对象。      |
| exec           | kubectl exec POD [-c CONTAINER] [-i] [-t] [flags] [– COMMAND [args…]] | 在Pod的容器中执行一个命令。                                 |
| explain        | kubectl explain [–include-extended-apis=true] [–recursive=false] [flags] | 获取Pod、Node和服务等资源对象的文档。                       |
| expose         | kubectl expose (-f FILENAME \| TYPE NAME \| TYPE/NAME) [–port=port]  [–protocol=TCP\|UDP] [–target-port=number-or-name] [–name=name]  [—-external-ip=external-ip-of-service] [–type=type] [flags] | 为副本控制器、服务或Pod等暴露一个新的服务。                 |
| get            | kubectl get (-f FILENAME \| TYPE [NAME \| /NAME \| -l label]) [–watch] [–sort-by=FIELD] [[-o \| –output]=OUTPUT_FORMAT] [flags] | 列出一个或多个资源。                                        |
| label          | kubectl label (-f FILENAME \| TYPE NAME \| TYPE/NAME) KEY_1=VAL_1 … KEY_N=VAL_N [–overwrite] [–all] [–resource-version=version] [flags] | 添加或更新一个或者多个资源对象的标签。                      |
| logs           | kubectl logs POD [-c CONTAINER] [–follow] [flags]            | 显示Pod中一个容器的日志。                                   |
| patch          | kubectl patch (-f FILENAME \| TYPE NAME \| TYPE/NAME) –patch PATCH [flags] | 使用策略合并补丁过程更新资源对象中的一个或多个字段。        |
| port-forward   | kubectl port-forward POD [LOCAL_PORT:]REMOTE_PORT […[LOCAL_PORT_N:]REMOTE_PORT_N] [flags] | 将一个或多个本地端口转发到Pod。                             |
| proxy          | kubectl proxy [–port=PORT] [–www=static-dir] [–www-prefix=prefix] [–api-prefix=prefix] [flags] | 为kubernetes API服务器运行一个代理。                        |
| replace        | kubectl replace -f FILENAME                                  | 从文件或stdin中替换资源对象。                               |
| rolling-update | kubectl rolling-update OLD_CONTROLLER_NAME ([NEW_CONTROLLER_NAME] –image=NEW_CONTAINER_IMAGE \| -f NEW_CONTROLLER_SPEC) [flags] | 通过逐步替换指定的副本控制器和Pod来执行滚动更新。           |
| run            | kubectl run NAME –image=image [–env=”key=value”] [–port=port]  [–replicas=replicas] [–dry-run=bool] [–overrides=inline-json] [flags] | 在集群上运行一个指定的镜像。                                |
| scale          | kubectl scale (-f FILENAME \| TYPE NAME \| TYPE/NAME) –replicas=COUNT  [–resource-version=version] [–current-replicas=count] [flags] | 扩宿容副本集的数量。                                        |
|                |                                                              |                                                             |
| version        | kubectl version [–client] [flags]                            | 显示运行在客户端和服务器端的Kubernetes版本。                |

#  kubectl输出选项

kubectl默认的输出格式为纯文本格式，可以通过-o或者–output字段指定命令的输出格式。

```
$ kubectl [command] [TYPE] [NAME] -o=<output_format>
```

#  

# kubernetes资源对象类型

在kubernetes中，提供了很多的资源对象，开发和运维人员可以通过这些对象对容器进行编排。在下表中，是kubectl所支持的资源对象类型，以及它们的缩略别名：

| 资源对象类型               | 缩略别名 |
| -------------------------- | -------- |
| apiservices                |          |
| certificatesigningrequests | csr      |
| clusters                   |          |
| clusterrolebindings        |          |
| clusterroles               |          |
| componentstatuses          | cs       |
| configmaps                 | cm       |
| controllerrevisions        |          |
| cronjobs                   |          |
| customresourcedefinition   | crd      |
| daemonsets                 | ds       |
| deployments                | deploy   |
| endpoints                  | ep       |
| events                     | ev       |
| horizontalpodautoscalers   | hpa      |
| ingresses                  | ing      |
| jobs                       |          |
| limitranges                | limits   |
| namespaces                 | ns       |
| networkpolicies            | netpol   |
| nodes                      | no       |
| persistentvolumeclaims     | pvc      |
| persistentvolumes          | pv       |
| poddisruptionbudget        | pdb      |
| podpreset                  |          |
| pods                       | po       |
| podsecuritypolicies        | psp      |
| podtemplates               |          |
| replicasets                | rs       |
| replicationcontrollers     | rc       |
| resourcequotas             | quota    |
| rolebindings               |          |
| roles                      |          |
| secrets                    |          |
| serviceaccounts            | sa       |
| services                   | svc      |
| statefulsets               |          |
| storageclasses             |          |

# kubectl安装部署

1、安装kubectl

```
sudo apt-get update && sudo apt-get install -y apt-transport-https

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

sudo touch /etc/apt/sources.list.d/kubernetes.list 

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl
```

其他操作系统下安装kubectl，请参考：https://kubernetes.io/docs/tasks/tools/install-kubectl/#tabset-2

2、配置kubeconfig文件

RKE会在配置文件所在的目录下部署一个本地文件，该文件中包含kube配置信息以连接到新生成的群集。

默认情况下，kube配置文件被称为.kube_config_cluster.yml。将这个文件复制到你的本地~/.kube/config，就可以在本地使用kubectl了。

需要注意的是，部署的本地kube配置名称是和集群配置文件相关的。例如，如果您使用名为mycluster.yml的配置文件，则本地kube配置将被命名为.kube_config_mycluster.yml。

3、手动设置Kubectl 上下文和配置

设置 kubectl 与其通信的 Kubernetes 集群，以及修改配置信息。

```html
$ kubectl config view # 显示合并的 kubeconfig 设置。

# 同时使用多个 kubeconfig 文件，并且查看合并的配置


$ KUBECONFIG=~/.kube/config:~/.kube/kubconfig2 kubectl config view


# 查看名称为 “e2e” 的用户的密码

$ kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'


$ kubectl config current-context              # 显示当前上下文

$ kubectl config use-context my-cluster-name  # 设置默认的上下文为 my-cluster-name

# 在 kubeconf 中添加一个支持基本鉴权的新集群。


$ kubectl config set-credentials kubeuser/foo.kubernetes.com --username=kubeuser --password=kubepassword

# 使用特定的用户名和命名空间设置上下文。

$ kubectl config set-context gce --user=cluster-admin --namespace=foo \

  && kubectl config use-context gce
```

4、验证

执行kubectl命令，获取nodes的信息：

```html
$ kubectl get nodes
```

# kubectl的常用命令示例

在此部分将提供常用命令的示例，已帮助您快速了解和试用kubectl。

1、kubectl create命令

此命令通过文件或者stdin创建一个资源对象，假设这里存在一个nginx部署的YAML配置文件，可以通过执行下面的命令创建部署对象。

```html
$ kubectl create -f nginx-deployment.yaml
```

nginx部署的YAML配置文件的示例代码如下：

```html
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2

kind: Deployment

metadata:

  name: nginx

spec:

  replicas: 10

  selector:

    matchLabels:

      app: nginx

  revisionHistoryLimit: 2

  template:

    metadata:

      labels:

        app: nginx

    spec:

      imagePullSecrets:

        - name: dc-hspfd

      containers:

      # 应用的镜像

      - image: nginx

        name: nginx

        imagePullPolicy: IfNotPresen

        # 应用的内部端口

        ports:

        - containerPort: 80

          name: nginx80

        # 持久化挂接位置，在docker中      

        volumeMounts:

        - mountPath: /usr/share/nginx/html

          name: nginx-data

        - mountPath: /etc/nginx

          name: nginx-conf

      volumes:

      # 宿主机上的目录


      - name: nginx-data


        nfs:

          path: /nfs/nginx

          server: 192.168.1.10

      - name: nginx-conf

        nfs:

          path: /k8s-nfs/nginx/conf

          server: 192.168.1.10
```

Kubernetes 清单可以用 json 或 yaml 来定义。使用的文件扩展名包括 .yaml, .yml 和 .json。

```html
$ kubectl create -f ./my-manifest.yaml           # 创建资源


$ kubectl create -f ./my1.yaml -f ./my2.yaml     # 从多个文件创建资源



$ kubectl create -f ./dir                        # 通过目录下的所有清单文件创建资源



$ kubectl create -f https://git.io/vPieo         # 使用 url 获取清单创建资源



$ kubectl run nginx --image=nginx                # 开启一个 nginx 实例



$ kubectl explain pods,svc                       # 获取 pod 和服务清单的描述文档


# 通过标准输入创建多个 YAML 对象


$ cat <<EOF | kubectl create -f -


apiVersion: v1

kind: Pod

metadata:

  name: busybox-sleep

spec:

  containers:

  - name: busybox

    image: busybox

    args:

    - sleep

    - "1000000"
---
apiVersion: v1

kind: Pod

metadata:

  name: busybox-sleep-less

spec:

  containers:

  - name: busybox

    image: busybox

    args:

    - sleep

    - "1000"

EOF

# 使用多个 key 创建一个 secret


$ cat <<EOF | kubectl create -f -


apiVersion: v1


kind: Secret


metadata:


  name: mysecret


type: Opaque

data:

  password: $(echo -n "s33msi4" | base64)

  username: $(echo -n "jane" | base64)

EOF

```

2、kubectl get 命令

通过此命令列出一个或多个资源对象，在这里通过kubectl get命令获取default命名空间下的所有部署。

```
kubectl get deployment
# 具有基本输出的 get 命令

$ kubectl get services                          # 列出命名空间下的所有 service


$ kubectl get pods --all-namespaces             # 列出所有命名空间下的 pod


$ kubectl get pods -o wide                      # 列出命名空间下所有 pod，带有更详细的信息


$ kubectl get deployment my-dep                 # 列出特定的 deployment


$ kubectl get pods --include-uninitialized      # 列出命名空间下所有的 pod，包括未初始化的对象

# 有详细输出的 describe 命令

$ kubectl describe nodes my-node

$ kubectl describe pods my-pod

$ kubectl get services --sort-by=.metadata.name # List Services Sorted by Name

# 根据重启次数排序，列出所有 pod



$ kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'


# 查询带有标签 app=cassandra 的所有 pod，获取它们的 version 标签值


$ kubectl get pods --selector=app=cassandra rc -o \


  jsonpath='{.items[*].metadata.labels.version}'

# 获取命名空间下所有运行中的 pod


$ kubectl get pods --field-selector=status.phase=Running

# 所有所有节点的 ExternalIP


$ kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'

# 列出输出特定 RC 的所有 pod 的名称


# "jq" 命令对那些 jsonpath 看来太复杂的转换非常有用，可以在这找到：https://stedolan.github.io/jq/



$ sel=${$(kubectl get rc my-rc --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')%?}


$ echo $(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})


# 检查那些节点已经 ready


$ JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \

 && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"

# 列出某个 pod 目前在用的所有 Secret


$ kubectl get pods -o json | jq '.items[].spec.containers[].env[]?.valueFrom.secretKeyRef.name' | grep -v null | sort | uniq

# 列出通过 timestamp 排序的所有 Event


$ kubectl get events --sort-by=.metadata.creationTimestamp
```

3、kubectl describe命令

此命令用于显示一个或多个资源对象的详细信息，在这里通过获取上述nginx部署的信息。

```html
$ kubectl describe deployments/nginx
```

4、kubectl exec命令

此命令用于在Pod中的容器上执行一个命令，此处在nginx的一个容器上执行/bin/bash命令。

```html
$ kubectl exec -it nginx-c5cff9dcc-dr88w /bin/bash
```

5、kubectl logs命令

此命令用于获取Pod中一个容器的日志信息，此处获取nginx一个容器的日志信息。

```html
$ kubectl logs nginx-c5cff9dcc-dr88w
```

6、kubectl delete命令

此命令用于删除集群中已存在的资源对象，可以通过指定名称、标签选择器、资源选择器等。

```html
$ kubectl delete -f ./pod.json                                              # 使用 pod.json 中指定的类型和名称删除 pod


$ kubectl delete pod,service baz foo                                        # 删除名称为 "baz" 和 "foo" 的 pod 和 service


$ kubectl delete pods,services -l name=myLabel                              # 删除带有标签 name=myLabel 的 pod 和 service


$ kubectl delete pods,services -l name=myLabel --include-uninitialized      # 删除带有标签 name=myLabel 的 pod 和 service，包括未初始化的对象


$ kubectl -n my-ns delete po,svc --all                                      # 删除命名空间 my-ns 下所有的 pod 和 service，包括未初始化的对象
```

7、kubectl rolling-update 命令

此命令用于滚动更新，对镜像、端口等的更新

```html
$ kubectl rolling-update frontend-v1 -f frontend-v2.json           # 滚动更新 pod：frontend-v1


$ kubectl rolling-update frontend-v1 frontend-v2 --image=image:v2  # 变更资源的名称并更新镜像


$ kubectl rolling-update frontend --image=image:v2                 # 更新 pod 的镜像


$ kubectl rolling-update frontend-v1 frontend-v2 --rollback        # 中止进行中的过程


$ cat pod.json | kubectl replace -f -                              # 根据传入标准输入的 JSON 替换一个 pod


# 强制替换，先删除，然后再重建资源。会导致服务中断。

$ kubectl replace --force -f ./pod.json

# 为副本控制器（rc）创建服务，它开放 80 端口，并连接到容器的 8080 端口

$ kubectl expose rc nginx --port=80 --target-port=8000
```

8、kubectl patch命令

```html
$ kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}' # 部分更新节点


# 更新容器的镜像，spec.containers[*].name 是必需的，因为它们是一个合并键



$ kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# 使用带有数组位置信息的 json 修补程序更新容器镜像


$ kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'

# 使用带有数组位置信息的 json 修补程序禁用 deployment 的 livenessProbe


$ kubectl patch deployment valid-deployment  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

# 增加新的元素到数组指定的位置中


$ kubectl patch sa default --type='json' -p='[{"op": "add", "path": "/secrets/1", "value": {"name": "whatever" } }]'
```

9、kubectl edit命令

```html
$ kubectl edit svc/docker-registry                      # 编辑名称为 docker-registry 的 service



$ KUBE_EDITOR="nano" kubectl edit svc/docker-registry   # 使用 alternative 编辑器
```

10、kubectl scale命令

```html
$ kubectl scale --replicas=3 rs/foo                                 # 缩放名称为 'foo' 的 replicaset，调整其副本数为 3



$ kubectl scale --replicas=3 -f foo.yaml                            # 缩放在 "foo.yaml" 中指定的资源，调整其副本数为 3



$ kubectl scale --current-replicas=2 --replicas=3 deployment/mysql  # 如果名称为 mysql 的 deployment 目前规模为 2，将其规模调整为 3



$ kubectl scale --replicas=5 rc/foo rc/bar rc/baz                   # 缩放多个副本控制器
```

11、与运行中的 pod 交互

```html
$ kubectl logs my-pod                                 # 转储 pod 日志到标准输出



$ kubectl logs my-pod -c my-container                 # 有多个容器的情况下，转储 pod 中容器的日志到标准输出



$ kubectl logs -f my-pod                              # pod 日志流向标准输出



$ kubectl logs -f my-pod -c my-container              # 有多个容器的情况下，pod 中容器的日志流到标准输出



$ kubectl run -i --tty busybox --image=busybox -- sh  # 使用交互的 shell 运行 pod



$ kubectl attach my-pod -i                            # 关联到运行中的容器



$ kubectl port-forward my-pod 5000:6000               # 在本地监听 5000 端口，然后转到 my-pod 的 6000 端口



$ kubectl exec my-pod -- ls /                         # 1 个容器的情况下，在已经存在的 pod 中运行命令



$ kubectl exec my-pod -c my-container -- ls /         # 多个容器的情况下，在已经存在的 pod 中运行命令



$ kubectl top pod POD_NAME --containers               # 显示 pod 及其容器的度量
```

12、与 node 和集群交互

```html
$ kubectl cordon my-node                                                # 标记节点 my-node 为不可调度

$ kubectl drain my-node                                                 # 准备维护时，排除节点 my-node

$ kubectl uncordon my-node                                              # 标记节点 my-node 为可调度

$ kubectl top node my-node                                              # 显示给定节点的度量值

$ kubectl cluster-info                                                  # 显示 master 和 service 的地址

$ kubectl cluster-info dump                                             # 将集群的当前状态转储到标准输出

$ kubectl cluster-info dump --output-directory=/path/to/cluster-state   # 将集群的当前状态转储到目录 /path/to/cluster-state

# 如果带有该键和效果的污点已经存在，则将按指定的方式替换其值


$ kubectl taint nodes foo dedicated=special-user:NoSchedule
```

 

# 参考资料

1.《kubectl Usage Conventions》地址：https://kubernetes.io/docs/reference/kubectl/conventions/

2.《kubectl Commands》地址：https://kubernetes.io/docs/reference/kubectl/kubectl-cmds/

3.《Overview of kubectl》地址：https://kubernetes.io/docs/reference/kubectl/overview/