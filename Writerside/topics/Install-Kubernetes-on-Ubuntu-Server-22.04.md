# Install Kubernetes on Ubuntu Server 22.04

## 参考文档

[安装 kubeadm](https://kubernetes.io/zh-cn/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## 公共部分

### 设置root密码

`sudo passwd root`

### 设置hostname

`sudo hostnamectl set-hostname k8s`

`sudo vim /etc/hostname`

`sudo vim /etc/hosts`

`sudo netplan apply`

### 卸载系统自带的`snapd`

`sudo systemctl stop snapd snapd.socket`

`sudo apt autoremove --purge -y snapd`

### 时间同步和时区设置

`sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

`sudo timedatectl set-timezone Asia/Shanghai`    // 设置时区为上海

`sudo bash -c "echo 'Asia/Shanghai' > /etc/timezone"`

`sudo apt install ntpdate`

`sudo ntpdate ntp1.aliyun.com`

`timedatectl set-local-rtc 0`    // 将当前UTC时间写入硬件时间

`sudo systemctl restart rsyslog.service cron.service`    // 重启依赖于系统时间的服务

`date -R`    // 查看确认当前时间

### 关闭swap分区

`sudo swapon --show` 或 `free -h`

如果启用了Swap分区则会显示Swap分区的总大小和使用情况

禁用swap：

`sudo swapoff -a`

然后删除swap文件：

`sudo rm /swap.img`

接下来修改fstab文件，以便在系统重新启动后不会重新创建Swap分区文件。

注释或删除`/etc/fstab`的：

`/swap.img none swap sw 0 0`

再次运行`sudo swapon --show`检查是否已禁用，如果禁用的话该命令应该没有输出

### **允许 iptables 检查桥接流量**

`sudo modprobe overlay && sudo modprobe br_netfilter`

持久化加载上述两个模块，避免重启失效。

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

通过运行 `lsmod | grep br_netfilter` 来验证 br_netfilter 模块是否已加载

通过运行`lsmod | grep overlay` 来验证 overlay模块是否已加载

修改内核参数，确保二层的网桥在转发包时也会被iptables的FORWARD规则所过滤

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF
```

应用 sysctl 参数而不重新启动

`sudo sysctl --system`

[K8s为啥要启用bridge-nf-call-iptables内核参数？用案例给你讲明白 - 技术颜良 - 博客园](https://www.cnblogs.com/cheyunhua/p/17334812.html)

[开启bridge功能是什么意思（k8s安装时开启bridge-nf-call-iptables）](https://www.tangshanwei.com/n/23948.html)

### Kernel参数调整

```shell
# 1.Kernel 参数调整
mkdir ~/k8s-init/
 
cat > ~/k8s-init/kubernetes-sysctl.conf <<EOF
# iptables 网桥模式开启
net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-ip6tables=1
# 禁用 ipv6 协议
net.ipv6.conf.all.disable_ipv6=1
# 启用ipv4转发
net.ipv4.ip_forward=1 
# net.ipv4.tcp_tw_recycle=0 #Ubuntu 没有参数
# 禁止使用 swap 空间，只有当系统 OOM 时才允许使用它
vm.swappiness=0
# 不检查物理内存是否够用
vm.overcommit_memory=1
# 不启 OOM
vm.panic_on_oom=0 
# 文件系统通知数(根据内存大小和空间大小配置)
fs.inotify.max_user_instances=8192
fs.inotify.max_user_watches=1048576
# 文件件打开句柄数
fs.file-max=52706963
fs.nr_open=52706963
net.netfilter.nf_conntrack_max=2310720
# tcp keepalive 相关参数配置
net.ipv4.tcp_keepalive_time = 600
net.ipv4.tcp_keepalive_probes = 3
net.ipv4.tcp_keepalive_intvl =15
net.ipv4.tcp_max_tw_buckets = 36000
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_max_orphans = 327680
net.ipv4.tcp_orphan_retries = 3
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 16384
# net.ipv4.ip_conntrack_max = 65536 # Ubuntu 没有参数
net.ipv4.tcp_timestamps = 0
net.core.somaxconn = 16384
EOF
 
sudo cp ~/k8s-init/kubernetes-sysctl.conf  /etc/sysctl.d/99-kubernetes.conf
sudo sysctl -p /etc/sysctl.d/99-kubernetes.conf
 
# 2.nftables 模式切换
# 在 Linux 中 nftables 当前可以作为内核 iptables 子系统的替代品,该工具可以充当兼容性层其行为类似于 iptables 但实际上是在配置 nftables。
sudo apt list | grep "nftables/jammy"
# nftables/focal 0.9.3-2 amd64
# python3-nftables/focal 0.9.3-2 amd64
 
# iptables 旧模式切换 (nftables 后端与当前的 kubeadm 软件包不兼容, 它会导致重复防火墙规则并破坏 kube-proxy, 所则需要把 iptables 工具切换到“旧版”模式来避免这些问题)
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```

### 安装Docker

[Install Docker on Ununtu Server](https://docs.craft.do/editor/d/541b7c88-d193-df3a-a475-0ade82a7d9d3/3901DD47-7E2C-47AF-9695-DB12C9478E30)

### 安装Kubernetes {id="kubernetes_1"}

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl
# (1) gpg 签名下载导入
curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
# (2) Kubernetes 安装源
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
// Pin the version
sudo apt-mark hold kubelet kubeadm kubectl 
# (4) 安装指定版本
sudo apt -y install kubelet=1.19.10-00  kubeadm=1.19.10-00 kubectl=1.19.10-00
```

查看版本

`kubectl version --client && kubeadm version && kubelet --version`

`sudo systemctl enable kubelet`

```bash
# Kubernetes1.24版本及以上移除了对docker的直接集成，使用containerd实现
sudo vim /etc/containerd/config.toml
# 注释这行
disabled_plugins : ["cri"]

sudo systemctl restart containerd
```

**修改运行时containerd 配置**

生成containerd的默认配置文件：

`containerd config default | sudo tee /etc/containerd/config.toml`

```bash
// Line 61
// 注意最后的3.9
sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.9"
...
// Line 114
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
Root = ""
ShimCgroup = ""
// 修改这行
SystemdCgroup = true
```

`sudo systemctl restart containerd`

`sudo systemctl enable containerd`

查看镜像版本号

`kubeadm config images list`

---

## ::Master::节点部分

生成初始化配置信息：

`kubeadm config print init-defaults > kubeadm.conf`

修改kubeadm.conf配置:

```bash
// Line 17
  name: k8s-master // 修改master节点主机名
// Line 12
localAPIEndpoint:
  advertiseAddress: 192.168.5.51  // 修改为master节点IP地址
  bindPort: 6443
// Line 30
imageRepository: registry.aliyuncs.com/google_containers. // 修改阿里云源
// Line 31
kubernetesVersion: v1.28.2  // 增加，版本与实际版本对应
// Line 36
networking:
  dnsDomain: cluster.local
  podSubnet: 10.244.0.0/16  // 增加
  serviceSubnet: 10.96.0.0/12
```

### 初始化集群

`sudo kubeadm init --config=kubeadm.conf --v=5`

```bash
# 输出
[init] Using Kubernetes version: v1.19.16
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
...
Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.65.145:6443 --token mj7yh1.kcsms343yksi1k8l \
    --discovery-token-ca-cert-hash sha256:06d33735dffa06b7de28e7837297fb982354f1667cc7d250e2a3dfdc468256cb
```

### 为用户配置 kubectl

run as current user without sudo

`su username`

`mkdir -p $HOME/.kube`

`sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

`sudo chown $(id -u):$(id -g) $HOME/.kube/config`

Node节点如需要则同样执行，拷贝Master节点中的`/etc/kubernetes/admin.conf`

`scp -r $HOME/.kube node1:$HOME`

`…`

> ~/.kube 文件夹包含了集群的配置信息，在每一个具有该文件夹的节点上将自动识别集群信息，可以运行`kubectl相关操作`

### 安装Master节点网络 插件 fannel

`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`

or

`wget` [`https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml`](https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml)

`kubectl apply -f ./kube-flannel.yml`

---

## 添加Node

**Master节点执行**

自动打印join命令：

`kubeadm token create --print-join-command`

或分别查看discovery-token-ca-cert-hash

```bash
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
   openssl dgst -sha256 -hex | sed 's/^.* //'
```

查看token

`kubeadm token list`

**在Node节点执行命令，将其注册到Cluster中**

token和discovery-token-ca-cert-hash来自master节点初始化集群完成时的输出

```bash
kubeadm join 192.168.65.145:6443 --token mj7yh1.kcsms343yksi1k8l \
    --discovery-token-ca-cert-hash sha256:06d33735dffa06b7de28e7837297fb982354f1667cc7d250e2a3dfdc468256cb
```

## 连接多个Kubernetes集群

`～/.kube/config` 多个config文件

```bash
KUBECONFIG=~/.kube/config:~/.kube/config2222 kubectl config view --merge --flatten >> ~/.kube/config

export KUBECONFIG=~/.kube/config
```

验证：

`kubectl config get-contexts`

切换集群：

`kubectl config use-context <context-name>`

[配置kubectl连接到不同的集群](https://zhuanlan.zhihu.com/p/669603168)