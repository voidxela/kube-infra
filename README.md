# Kubernetes general infra

## Deploying
```bash
sudo sysctl vm.nr_hugepages=1024
echo 'vm.nr_hugepages=1024' | sudo tee -a /etc/sysctl.conf
sudo apt install -y linux-modules-extra-$(uname -r)
sudo modprobe nvme_tcp
echo 'nvme-tcp' | sudo tee -a /etc/modules-load.d/microk8s-mayastor.conf
(microk8s stop && microk8s start) || sudo snap install microk8s --classic
microk8s add-node  # as needed, follow instructions
microk8s enable community
microk8s enable dns
microk8s enable rbac
microk8s enable mayastor
microk8s enable cert-manager
microk8s enable easyhaproxy
microk8s enable portainer
alias k="microk8s kubectl"  # add to ~/.bash_aliases
k apply -f cluster-issuer.yml
k apply -f portainer-ingress.yml
k apply -f rook-operator.yml
```