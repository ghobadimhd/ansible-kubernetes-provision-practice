Kubernetes provision
=========

This is simple playbook for deploying kubernetes cluster.

Inventory example
-----------------

```ini

[kubernetes_cluster:vars]
# defaults file for kubernetes
ansible_user=vagrant 
ansible_become=true
kubernetes_kubernetesVersion=1.18.8
kubernetes_serviceSubnet="10.96.0.0/12"
kubernetes_podSubnet="10.20.0.0/16"
kubernetes_clustercidr="10.20.0.0/16"
kubernetes_cluster_name="cluster.local"
kubernetes_imageRepository="k8s.gcr.io"
kubernetes_loadbalancer_port=8443
kubernetes_loadbalancer_address=192.168.11.200
kubernetes_apt_repository="deb https://apt.kubernetes.io/ kubernetes-xenial main"
kubernetes_apt_docker_repository="deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
kubernetes_kubeadm_init_extra_args="--ignore-preflight-errors=NumCPU"

[kubernetes_cluster:children]
kubernetes_loadbalancer
kubernetes_nodes

[kubernetes_nodes:children]
kubernetes_masters
kubernetes_workers

[kubernetes_masters]
master-1 ansible_host=192.168.11.195
master-2 ansible_host=192.168.11.197
master-3 ansible_host=192.168.11.198

[kubernetes_workers]
worker-1 ansible_host=192.168.11.199

[kubernetes_loadbalancer]
haproxy ansible_host=192.168.11.200

[kubernetes_loadbalancer:vars]
kubernetes_loadbalancer_admin_pass=123
kubernetes_loadbalancer_stats_port=8080
```

