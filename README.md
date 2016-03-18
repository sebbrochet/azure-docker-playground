# A few docker instances behind a jumpbox on Azure...

## VMs creation
* VMs creation is [here](vm_creation)

## SSH connection configuration
Once your VMs are created, you need some configuration to access them:   
* Update your local `/etc/hosts` file with the private IP of all the VMs
  ```
  # add these lines at the end of the file
  # you may need to check if private IP are correct!
  10.0.0.6    jumpbox.azure
  10.0.0.5    docker1.azure
  10.0.0.4    docker2.azure
  10.0.0.7    docker3.azure
  ```
* Create a local file called `~/.ssh/config` to configure the jumpbox as a proxy to connect to all VMs
* Paste the following code in it   
  ```
  Host *.azure
  ProxyCommand ssh <adminUsername>@<dnsLabelPrefix>.<location>.cloudapp.azure.com -W %h:%p
  ```
* Update the `/etc/hosts file` on your jumpbox as described above   
  ```
  ssh adminUsername@<dnsLabelPrefix>.<location>.cloudapp.azure.com
  sudo vi /etc/hosts
  # paste the few line above at then end of the file
  ```
* You should be now able to connect to all the VM with SSH   
  `ssh <adminUsername>@<vm_name>.azure`

## Provisioning  
* Provisioning ... to be done!
