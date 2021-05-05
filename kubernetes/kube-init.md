##### install kubeadm

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

sudo su -

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list

apt-get update && apt-get install -y kubeadm

#####

##### prepare docker

https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker

# Setup daemon.
sudo mkdir /etc/docker
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

# Restart docker.
sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker

#####

##### init cluster

sudo kubeadm init --token-ttl=0 --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

### flannel

or flannel to work correctly, you must pass --pod-network-cidr=10.244.0.0/16 to kubeadm init.

Set /proc/sys/net/bridge/bridge-nf-call-iptables to 1 by running sysctl net.bridge.bridge-nf-call-iptables=1 to pass bridged IPv4 traffic to iptables’ chains. This is a requirement for some CNI plugins to work, for more information please see here.

Make sure that your firewall rules allow UDP ports 8285 and 8472 traffic for all hosts participating in the overlay network. see here .

Note that flannel works on amd64, arm, arm64, ppc64le and s390x under Linux. Windows (amd64) is claimed as supported in v0.11.0 but the usage is undocumented.

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml



### join

sudo kubeadm join 192.168.2.150:6443 --token wzr5w1.vybji14gfkmjveon \
        --discovery-token-ca-cert-hash sha256:e045442c65f88dec2fce8b765360e88f114299ddbed6caff6da96d29defda86c

### reset

sudo kubeadm reset

sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT

sudo iptables -F

kubeadm join 192.168.2.150:6443 --token qo77ad.1hrl38rop3o84r4p \
    --discovery-token-ca-cert-hash sha256:fbd84fddea9d45d9f978a667a5446e18df04ec1c206a47a334f04dd31dd22433
