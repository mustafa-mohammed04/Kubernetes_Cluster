## Make VMs as a Vagrant 
1- Install libvirt Package
## Update the Package List
``` bash
sudo apt-get update -y
```
## Install Libvirt and Required Packages
``` bash
sudo apt install -y libvirt-daemon libvirt-clients qemu qemu-kvm virt-manager bridge-utils
```
## Add Your User to the libvirt Group "Note: Replace $USER with your username if needed." and Verify
``` bash
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
sudo systemctl status libvirtd
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```
## Test the Setup "Open the Virtual Machine Manager" and check the connection to libvirt via CLI
``` bash
virt-manager
virsh list --all
```

2- Vagrant

## Install Vagrant and PreRequisities 
``` bash
sudo apt-get update -y
sudo apt install -y virtualbox curl gnupg software-properties-common
```
## Add the Vagrant Official Repository "Import the Vagrant public key" and Add the HashiCorp repository
``` bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
dpkg -i <vagrant-release>
sudo apt install -f 
vagrant --version
```
## Install Vagrant Plugins "Optional"
``` bash
vagrant plugin install vagrant-vbguest
```

3- Create k8s-cluster using k3s depend on Vagrant 
## Create 3 directories to 3 Vagrant files
``` bash
mkdir vm-master
mkdir vm-worker-1
mkdir vm-worker-2
```
## Inside in vm-master dir to create Vagrantfile
``` bash
cd vm-master
vagrant init 
ls "Vagrantfile is existing after vagrant init"
vim Vagrantfile

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Specify the box to use
  config.vm.box = "generic/ubuntu2204"

  # Set the VM hostname
  config.vm.hostname = "Master.local"

  # Configure public network with a static IP and specify the correct interface
  config.vm.network "private_network", ip: "192.168.56.12", bridge: "wlo1"

  # Provider-specific configuration for Libvirt
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048         # Set memory to 2GB
    libvirt.cpus = 2              # Allocate 2 CPUs
    libvirt.disk_bus = "virtio"   # Use virtio for better disk performance
  end
end



vagrant validate                ## to Check 
vagrant up
vagrant ssh
```

## Inside in vm-worker-1 dir to create Vagrantfile
``` bash
cd vm-worker-1
vagrant init 
ls "Vagrantfile is existing after vagrant init"
vim Vagrantfile

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Specify the box to use
  config.vm.box = "generic/ubuntu2204"

  # Set the VM hostname
  config.vm.hostname = "agent-1"

  # Configure public network with a static IP and specify the correct interface
  config.vm.network "private_network", ip: "<ip address>", bridge: "<interface connection>"

  # Provider-specific configuration for Libvirt
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048         # Set memory to 2GB
    libvirt.cpus = 2              # Allocate 2 CPUs
    libvirt.disk_bus = "virtio"   # Use virtio for better disk performance
  end
end

vagrant validate                ## to Check 
vagrant up
vagrant ssh
```
## Inside in vm-worker-2 dir to create Vagrantfile
``` bash
cd vm-worker-2
vagrant init 
ls "Vagrantfile is existing after vagrant init"
vim Vagrantfile

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Specify the box to use
  config.vm.box = "generic/ubuntu2204"

  # Set the VM hostname
  config.vm.hostname = "agent-2"

  # Configure public network with a static IP and specify the correct interface
  config.vm.network "private_network", ip: "<ip address>", bridge: "<interface connection>"

  # Provider-specific configuration for Libvirt
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048         # Set memory to 2GB
    libvirt.cpus = 2              # Allocate 2 CPUs
    libvirt.disk_bus = "virtio"   # Use virtio for better disk performance
  end
end



vagrant validate                ## to Check 
vagrant up                    ## to actvate vagrant
vagrant ssh                  
```
## Install k3s repo on server machine"Note": k3s depend on SQL Lite as Key value store DATABASE if you want to change to etcd "cluster-init: true" in config.yaml
``` bash
mkdir -p /etc/rancher/k3s
cat cat /var/lib/rancher/k3s/server/token              "default token path"
vim /etc/rancher/k3s/config.yaml


token: <any-token if you want use a custom token not default>
node-ip: <ip of server>
cluster-init: true          ## to use ETCD DATABASE Store

curl https://get.k3s.io | sh -s 
systemctl status k3s.service
systemctl enable k3s.service
systemctl start k3s.service
```
## Install k3s agent 

## Install k3s repo on agent-1 or worker-1 machine

``` bash
mkdir -p /etc/rancher/k3s
vim /etc/rancher/k3s/config.yaml


token: <any-token if you want use a custom token not default like in server-node>
node-ip: <ip of agent-1 or worker-1>
server:https://<ip of server-node>:6443

curl https://get.k3s.io | sh -s agent
systemctl status k3s-agent.service
```

## Install k3s repo on agent-2 or worker-2 machine
``` bash
mkdir -p /etc/rancher/k3s
vim /etc/rancher/k3s/config.yaml

token: <any-token if you want use a custom token not default like in server-node>
node-ip: <ip of agent-2 or worker-2>
server:https://<ip of server-node>:6443

curl https://get.k3s.io | sh -s agent
systemctl status k3s-agent.service
```


