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
 ## Control     >>>> Hostname
 ## Worker      >>>> Hostname
 
 ``` bash
 mkdir vm_dir1
 mkdir vm_dir2
 
 cd vm_dir1
vagrant init
ls     >>>> Vagrantfile
vim Vagrantfile

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = "ubuntu/jammy64"


 
  config.vm.hostname = "Control"
  config.vm.network "private_network", ip: "192.168.56.12", hostname: true


  config.vm.provider :virtualbox do |vb|


   
    vb.customize [
      'modifyvm', :id,
      '--memory', '2048',
      '--cpus', '2'
    ]
  end

 
end



vagrant up
vagrant ssh
```

``` bash

cd vm_dir2
vagrant init
ls     >>>> Vagrantfile
vim Vagrantfile

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/jammy64"


  
  config.vm.hostname = "Worker"
  config.vm.network "private_network", ip: "192.168.56.13", hostname: true


  config.vm.provider :virtualbox do |vb|


   
    vb.customize [
      'modifyvm', :id,
      '--memory', '2048',
      '--cpus', '2'
    ]
  end

  
end




vagrant up
vagrant ssh

```


