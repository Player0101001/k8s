
4.roles
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/master=memeda
node/kmaster labeled
[root@kmaster ~]# kubectl get nodes kmaster --show-labels 
NAME      STATUS   ROLES                  AGE     VERSION   LABELS
kmaster   Ready    control-plane,master   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=kmaster,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=memeda,node.kubernetes.io/exclude-from-external-load-balancers=
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/control-plane-
node/kmaster unlabeled
[root@kmaster ~]# kubectl get nodes kmaster --show-labels 
NAME      STATUS   ROLES    AGE     VERSION   LABELS
kmaster   Ready    master   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=kmaster,kubernetes.io/os=linux,node-role.kubernetes.io/master=memeda,node.kubernetes.io/exclude-from-external-load-balancers=
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/master-
node/kmaster unlabeled
[root@kmaster ~]# kubectl get nodes kmaster --show-labels 
NAME      STATUS   ROLES    AGE     VERSION   LABELS
kmaster   Ready    <none>   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=kmaster,kubernetes.io/os=linux,node.kubernetes.io/exclude-from-external-load-balancers=
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/master=
node/kmaster labeled
[root@kmaster ~]# kubectl get nodes kmaster --show-labels 
NAME      STATUS   ROLES    AGE     VERSION   LABELS
kmaster   Ready    master   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=kmaster,kubernetes.io/os=linux,node-role.kubernetes.io/master=,node.kubernetes.io/exclude-from-external-load-balancers=
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/master-
node/kmaster unlabeled
[root@kmaster ~]# kubectl label nodes kmaster node-role.kubernetes.io/control-plane=
node/kmaster labeled
[root@kmaster ~]# kubectl get nodes kmaster --show-labels 
NAME      STATUS   ROLES           AGE     VERSION   LABELS
kmaster   Ready    control-plane   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=kmaster,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=

[root@kmaster ~]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d21h   v1.26.0
knode1    Ready    <none>          5d21h   v1.26.0
knode2    Ready    <none>          5d21h   v1.26.0
[root@kmaster ~]# 
[root@kmaster ~]# kubectl label nodes knode1 node-role.kubernetes.io/node1=
node/knode1 labeled
[root@kmaster ~]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d21h   v1.26.0
knode1    Ready    node1           5d21h   v1.26.0
knode2    Ready    <none>          5d21h   v1.26.0
[root@kmaster ~]# kubectl label nodes knode2 node-role.kubernetes.io/node2=
node/knode2 labeled
[root@kmaster ~]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d21h   v1.26.0
knode1    Ready    node1           5d21h   v1.26.0
knode2    Ready    node2           5d21h   v1.26.0

Annotations 注释
[root@kmaster ~]# kubectl annotate nodes knode1 zheshiyiduanzhushi:hehehe
error: at least one annotation update is required
[root@kmaster ~]# kubectl annotate nodes knode1 zhe.shi.yi.duanzhushi:hehehe
error: at least one annotation update is required
[root@kmaster ~]# kubectl annotate nodes knode1 zheshiyiduanzhushi=hehehe
node/knode1 annotated
[root@kmaster ~]# kubectl describe nodes knode1 |head -20
Name:               knode1
Roles:              node1
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=knode1
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/node1=
Annotations:        csi.volume.kubernetes.io/nodeid: {"csi.tigera.io":"knode1"}
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    projectcalico.org/IPv4Address: 192.168.100.181/24
                    projectcalico.org/IPv4VXLANTunnelAddr: 10.244.195.128
                    volumes.kubernetes.io/controller-managed-attach-detach: true
                    zheshiyiduanzhushi: hehehe
CreationTimestamp:  Wed, 22 Mar 2023 22:11:22 +0800
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  knode1

指定pod运行在某个节点上
[root@kmaster ~]# kubectl get nodes knode2 --show-labels 
NAME     STATUS   ROLES   AGE     VERSION   LABELS
knode2   Ready    node2   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=knode2,kubernetes.io/os=linux,node-role.kubernetes.io/node2=
[root@kmaster ~]# 
[root@kmaster ~]# kubectl label nodes knode2 disktype=ssdnvme
node/knode2 labeled
[root@kmaster ~]# kubectl get nodes knode2 --show-labels 
NAME     STATUS   ROLES   AGE     VERSION   LABELS
knode2   Ready    node2   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disktype=ssdnvme,kubernetes.io/arch=amd64,kubernetes.io/hostname=knode2,kubernetes.io/os=linux,node-role.kubernetes.io/node2=

[root@kmaster 328]# vim pod2.yaml 
[root@kmaster 328]# cat pod2.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod2
  name: pod2
spec:
  nodeSelector:
    disktype: ssdnvme
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: pod2
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
[root@kmaster 328]# kubectl get nodes knode2 --show-labels 
NAME     STATUS   ROLES   AGE     VERSION   LABELS
knode2   Ready    node2   5d21h   v1.26.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disktype=ssdnvme,kubernetes.io/arch=amd64,kubernetes.io/hostname=knode2,kubernetes.io/os=linux,node-role.kubernetes.io/node2=
[root@kmaster 328]# 
[root@kmaster 328]# kubectl apply -f pod2.yaml 
pod/pod2 created
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             30m   10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (38m ago)   38m   10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             5s    10.244.69.198    knode2   <none>           <none>

如果你在pod里面定义nodeSelector标签的时候，如果定义了一个不存在的标签，请问会创建成功吗？
手工指定的nodeSelector ，要么指定对，要么不指定，指定错了，pod会挂起。
[root@kmaster 328]# vim pod3.yaml 
[root@kmaster 328]# cat pod3.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod3
  name: pod3
spec:
  nodeSelector:
    disktypee: ssdnvme
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: pod3
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
[root@kmaster 328]# kubectl apply -f pod3.yaml 
pod/pod3 created
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE     IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             33m     10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (41m ago)   41m     10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             2m31s   10.244.69.198    knode2   <none>           <none>
pod3          0/1     Pending   0             5s      <none>           <none>   <none>           <none>

[root@kmaster 328]# kubectl get pod
NAME          READY   STATUS    RESTARTS      AGE
pod1          1/1     Running   0             37m
pod1-knode1   1/1     Running   1 (45m ago)   44m
pod2          1/1     Running   0             6m22s
pod3          0/1     Pending   0             3m56s
[root@kmaster 328]# kubectl delete -f pod3.yaml 
pod "pod3" deleted
[root@kmaster 328]# kubectl get pod
NAME          READY   STATUS    RESTARTS      AGE
pod1          1/1     Running   0             37m
pod1-knode1   1/1     Running   1 (45m ago)   45m
pod2          1/1     Running   0             6m32s

cordon/drain/taint污点

cordon 告警警戒
--------------
比如有两个节点，当创建一个pod的时候，会根据scheduler调度算法，分布在不同的节点上。现在有knode1和knode2两个节点，如果knode1节点要维护或检查（新创建的pod不能再调度到我这个node1上了）。

[root@kmaster 328]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d21h   v1.26.0
knode1    Ready    node1           5d21h   v1.26.0
knode2    Ready    node2           5d21h   v1.26.0
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             41m   10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (49m ago)   49m   10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             10m   10.244.69.198    knode2   <none>           <none>
[root@kmaster 328]# kubectl co
completion  (Output shell completion code for the specified shell (bash, zsh, fish, or powershell))
config      (Modify kubeconfig files)
cordon      (Mark node as unschedulable)
[root@kmaster 328]# kubectl co
completion  (Output shell completion code for the specified shell (bash, zsh, fish, or powershell))
config      (Modify kubeconfig files)
cordon      (Mark node as unschedulable)
[root@kmaster 328]# kubectl cordon knode1
node/knode1 cordoned
[root@kmaster 328]# kubectl get nodes 
NAME      STATUS                     ROLES           AGE     VERSION
kmaster   Ready                      control-plane   5d21h   v1.26.0
knode1    Ready,SchedulingDisabled   node1           5d21h   v1.26.0
knode2    Ready                      node2           5d21h   v1.26.0
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             43m   10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (51m ago)   51m   10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             12m   10.244.69.198    knode2   <none>           <none>
[root@kmaster 328]# ls
pod1.yaml  pod2.yaml  pod3.yaml
[root@kmaster 328]# cp pod2.yaml pod4.yaml
[root@kmaster 328]# vim pod4.yaml 
[root@kmaster 328]# kubectl apply -f pod4.yaml 
pod/pod4 created
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             44m   10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (52m ago)   52m   10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             13m   10.244.69.198    knode2   <none>           <none>
pod4          1/1     Running   0             3s    10.244.69.199    knode2   <none>           <none>

取消cordon
[root@kmaster 328]# kubectl uncordon knode1
node/knode1 uncordoned
[root@kmaster 328]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d21h   v1.26.0
knode1    Ready    node1           5d21h   v1.26.0
knode2    Ready    node2           5d21h   v1.26.0
[root@kmaster 328]# cp pod4.yaml pod5.yaml
[root@kmaster 328]# vim pod5.yaml 
[root@kmaster 328]# kubectl apply -f pod5.yaml 
pod/pod5 created
[root@kmaster 328]# kubectl get pod -o wide
NAME          READY   STATUS    RESTARTS      AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1          1/1     Running   0             45m   10.244.69.197    knode2   <none>           <none>
pod1-knode1   1/1     Running   1 (53m ago)   53m   10.244.195.134   knode1   <none>           <none>
pod2          1/1     Running   0             14m   10.244.69.198    knode2   <none>           <none>
pod4          1/1     Running   0             69s   10.244.69.199    knode2   <none>           <none>
pod5          1/1     Running   0             4s    10.244.195.135   knode1   <none>           <none>

drain操作
它比刚才cordon多了一个动作。
cordon对于已经在该节点上运行的pod，不会驱逐（没有讲deploy，现阶段：理解为删除）。而drain多了一个驱逐的动作。包含两个动作（cordon/evicted）

[root@kmaster 328]# kubectl get pod -o wide
NAME   READY   STATUS    RESTARTS   AGE   IP               NODE     NOMINATED NODE   READINESS GATES
pod1   1/1     Running   0          50s   10.244.69.200    knode2   <none>           <none>
pod2   1/1     Running   0          39s   10.244.69.201    knode2   <none>           <none>
pod4   1/1     Running   0          33s   10.244.195.136   knode1   <none>           <none>
pod5   1/1     Running   0          3s    10.244.69.202    knode2   <none>           <none>
[root@kmaster 328]# 
[root@kmaster 328]# 
[root@kmaster 328]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d22h   v1.26.0
knode1    Ready    node1           5d21h   v1.26.0
knode2    Ready    node2           5d21h   v1.26.0
[root@kmaster 328]# kubectl drain knode2
node/knode2 cordoned
error: unable to drain node "knode2" due to error:[cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): calico-system/calico-node-rkp6j, calico-system/csi-node-driver-zzwvx, kube-system/kube-proxy-87qn9, cannot delete Pods declare no controller (use --force to override): default/pod1, default/pod2, default/pod5], continuing command...
There are pending nodes to be drained:
 knode2
cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): calico-system/calico-node-rkp6j, calico-system/csi-node-driver-zzwvx, kube-system/kube-proxy-87qn9
cannot delete Pods declare no controller (use --force to override): default/pod1, default/pod2, default/pod5

[root@kmaster 328]# kubectl drain knode2 --ignore-daemonsets --force
node/knode2 already cordoned
Warning: ignoring DaemonSet-managed Pods: calico-system/calico-node-rkp6j, calico-system/csi-node-driver-zzwvx, kube-system/kube-proxy-87qn9; deleting Pods that declare no controller: default/pod1, default/pod2, default/pod5
evicting pod default/pod5
evicting pod calico-apiserver/calico-apiserver-688757cbb5-9chzz
evicting pod calico-system/calico-typha-6cfbbcb7d4-hnn5p
evicting pod default/pod1
evicting pod default/pod2
pod/pod1 evicted
pod/pod5 evicted
pod/calico-apiserver-688757cbb5-9chzz evicted
pod/pod2 evicted
pod/calico-typha-6cfbbcb7d4-hnn5p evicted
node/knode2 drained
[root@kmaster 328]# kubectl get pod
NAME   READY   STATUS    RESTARTS   AGE
pod4   1/1     Running   0          7m3s

污点
----

总结：
cordon 警戒线（警告）：一旦设置了cordon，新的pod是不允许被调度的。
drain 驱逐：一旦设置了drain，不仅会cordon，还会evicted驱逐。（本意是把该节点上的pod删除掉，并在其他node上启动）
taint 污点：一但设置了taint，默认调度器会直接过滤掉，不会调度到该node上，但是可以通过 tolerations 关键字来强制的运行。

如何体现驱逐的概念？
[root@kmaster 328]# kubectl create deployment web1 --image nginx --dry-run=client -o yaml > web1.yaml
[root@kmaster 328]# ls
pod1.yaml  pod2.yaml  pod3.yaml  pod4.yaml  pod5.yaml  pod6.yaml  pod7.yaml  web1.yaml
[root@kmaster 328]# cat web1.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: web1
  name: web1
spec:
  replicas: 6
  selector:
    matchLabels:
      app: web1
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: web1
    spec:
      containers:
      - image: nginx
        imagePullPolicy: IfNotPresent
        name: nginx
        resources: {}
status: {}

[root@kmaster 328]# kubectl apply -f web1.yaml 
deployment.apps/web1 created
[root@kmaster 328]# kubectl get pod -o wide
NAME                    READY   STATUS              RESTARTS   AGE   IP       NODE     NOMINATED NODE   READINESS GATES
web1-74b86ff946-8z729   0/1     ContainerCreating   0          3s    <none>   knode1   <none>           <none>
web1-74b86ff946-b2n2m   0/1     ContainerCreating   0          3s    <none>   knode2   <none>           <none>
web1-74b86ff946-czlb6   0/1     ContainerCreating   0          3s    <none>   knode1   <none>           <none>
web1-74b86ff946-f7lhp   0/1     ContainerCreating   0          3s    <none>   knode2   <none>           <none>
web1-74b86ff946-z2tx7   0/1     ContainerCreating   0          3s    <none>   knode2   <none>           <none>
web1-74b86ff946-zm8gc   0/1     ContainerCreating   0          3s    <none>   knode1   <none>           <none>
[root@kmaster 328]# kubectl get pod -o wide
NAME                    READY   STATUS    RESTARTS   AGE   IP               NODE     NOMINATED NODE   READINESS GATES
web1-74b86ff946-8z729   1/1     Running   0          27s   10.244.195.139   knode1   <none>           <none>
web1-74b86ff946-b2n2m   1/1     Running   0          27s   10.244.69.206    knode2   <none>           <none>
web1-74b86ff946-czlb6   1/1     Running   0          27s   10.244.195.138   knode1   <none>           <none>
web1-74b86ff946-f7lhp   1/1     Running   0          27s   10.244.69.208    knode2   <none>           <none>
web1-74b86ff946-z2tx7   1/1     Running   0          27s   10.244.69.207    knode2   <none>           <none>
web1-74b86ff946-zm8gc   1/1     Running   0          27s   10.244.195.140   knode1   <none>           <none>

现在一共6个pod，node1和node2上分别运行了3个。

[root@kmaster 328]# kubectl get nodes 
NAME      STATUS   ROLES           AGE     VERSION
kmaster   Ready    control-plane   5d23h   v1.26.0
knode1    Ready    node1           5d23h   v1.26.0
knode2    Ready    node2           5d23h   v1.26.0
[root@kmaster 328]# 
[root@kmaster 328]# kubectl drain knode1
node/knode1 cordoned
error: unable to drain node "knode1" due to error:cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): calico-system/calico-node-9kmm4, calico-system/csi-node-driver-q5z7m, kube-system/kube-proxy-x88s7, continuing command...
There are pending nodes to be drained:
 knode1
cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): calico-system/calico-node-9kmm4, calico-system/csi-node-driver-q5z7m, kube-system/kube-proxy-x88s7

对knode1进行驱逐操作
[root@kmaster 328]# kubectl drain knode1 --ignore-daemonsets --force
node/knode1 already cordoned
Warning: ignoring DaemonSet-managed Pods: calico-system/calico-node-9kmm4, calico-system/csi-node-driver-q5z7m, kube-system/kube-proxy-x88s7
evicting pod tigera-operator/tigera-operator-54b47459dd-f56jg
evicting pod calico-apiserver/calico-apiserver-688757cbb5-tsqs8
evicting pod calico-system/calico-typha-6cfbbcb7d4-9t4ll
evicting pod default/web1-849556688-fsdcx
evicting pod default/web1-849556688-k6ntr
evicting pod default/web1-849556688-vv9d6
pod/tigera-operator-54b47459dd-f56jg evicted
pod/web1-849556688-k6ntr evicted
pod/web1-849556688-vv9d6 evicted
pod/calico-apiserver-688757cbb5-tsqs8 evicted
pod/web1-849556688-fsdcx evicted
pod/calico-typha-6cfbbcb7d4-9t4ll evicted
node/knode1 drained

看到原来运行在knode1上的pod都在node2上被创建了出来
[root@kmaster 328]# kubectl get pod -o wide
NAME                   READY   STATUS    RESTARTS   AGE   IP              NODE     NOMINATED NODE   READINESS GATES
web1-849556688-2z29z   1/1     Running   0          83s   10.244.69.209   knode2   <none>           <none>
web1-849556688-47rg8   1/1     Running   0          25s   10.244.69.212   knode2   <none>           <none>
web1-849556688-8nh64   1/1     Running   0          83s   10.244.69.211   knode2   <none>           <none>
web1-849556688-bms2h   1/1     Running   0          83s   10.244.69.210   knode2   <none>           <none>
web1-849556688-tbf75   1/1     Running   0          25s   10.244.69.215   knode2   <none>           <none>
web1-849556688-w8bl7   1/1     Running   0          25s   10.244.69.213   knode2   <none>           <none>
[root@kmaster 328]# 
[root@kmaster 328]# 
[root@kmaster 328]# ls
pod1.yaml  pod2.yaml  pod3.yaml  pod4.yaml  pod5.yaml  pod6.yaml  pod7.yaml  web1.yaml
