# Kubernetes_Cluster
## Vagrant Install
## Install Vbox as CLI to your distribution linux and Vagrant 

``` bash
sudo apt install virtualbox
virtualbox --version
virtualbox

sudo apt install vagrant
vagrant --version
vagrant
```

## Install Vbox as GUI
 >> Download any version  from vbox website 
 
 ``` bash 
 cd Downloads/
 ls
 sudo dpkg -i virtualbox-7.0_7.0.8-156879~Ubuntu~focal_amd64.deb
 sudo apt -f install 
 ```
 
 ## Make two Vagrant_Intances
 ## Create Two Directory
 
 ## In First Dir
 ``` bash
  mkdir vm_dir1
  mkdir vm_dir2
 ```
 
 ``` bash
 
cd vm_dir1
vagrant init
ls     >>>> Vagrantfile
vim Vagrantfile
```
``` ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = "ubuntu/jammy64"


 
  config.vm.hostname = "server"
  config.vm.network "public_network", ip: "192.168.56.12", hostname: true


  config.vm.provider :virtualbox do |vb|


   
    vb.customize [
      'modifyvm', :id,
      '--memory', '2048',
      '--cpus', '2'
    ]
  end

 
end
```
``` bash
vim /etc/hosts       
192.168.56.12 server
192.168.56.13 agent
```
``` bash
vagrant validate  >>  to check sentiques in Vagrantfile
vagrant up
vagrant ssh
```

## In Second Dir
``` bash

cd vm_dir2
vagrant init
ls     >>>> 

vim Vagrantfile
```
``` ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/jammy64"


  
  config.vm.hostname = "agent"
  config.vm.network "public_network", ip: "192.168.56.13", hostname: true


  config.vm.provider :virtualbox do |vb|


   
    vb.customize [
      'modifyvm', :id,
      '--memory', '2048',
      '--cpus', '2'
    ]
  end

  
end
```
``` bash
vim /etc/hosts       
192.168.56.13 agent
192.168.56.12 server
```
``` bash
vagrant validate       >> to check sentiques in Vagrantfile
vagrant up
vagrant ssh

```

## In K3s with Vagrant
## In Server-node
``` bash
mkdir -p /etc/rancher/k3s
vim /etc/rancher/k3s/config.yaml
```
``` yaml
token: anubis-k3s
node-ip: 192.168.56.12
```

``` bash
curl https://get.k3s.io | sh -s 
systemctl stats k3s.service
kubectl get nodes
kubectl get pods -A
```

## In agent-node
``` bash
mkdir -p /etc/rancher/k3s
vim /etc/rancher/k3s/config.yaml
```
``` yaml
server: https://192.168.56.12:6443
token: anubis-k3s
node-ip: 192.168.56.13
```

``` bash
curl https://get.k3s.io | sh -s agent
systemctl stats k3s-agent.service
kubectl get nodes
kubectl get pods -A
```

## To Attach k3s with etcd

``` bash
mkdir -p /etc/rancher/k3s
vim /etc/rancher/k3s/config.yaml
```
``` yaml
cluster-init: true
```
``` bash
curl https://get.k3s.io | sh -s
```
## to display logs
``` bash
journalctl -u k3s | gerp etcd
```
