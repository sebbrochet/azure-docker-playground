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
  # Be sure to update adminUsername, dnsLabelPrefix with the values you've used to create the VMs!
  # location is the azure region where you've created the VMs (i.e westeurope, ...)
  Host *.azure
  ProxyCommand ssh <adminUsername>@<dnsLabelPrefix>.<location>.cloudapp.azure.com -W %h:%p
  ```

* Update the `/etc/hosts file` on your jumpbox as described above   
  * Be sure to update adminUsername, dnsLabelPrefix with the values you've used to create the VMs!
    * location is the azure region where you've created the VMs (i.e westeurope, ...)
  * `ssh <adminUsername>@<dnsLabelPrefix>.<location>.cloudapp.azure.com`
  * `sudo vi /etc/hosts`
  * Paste the few lines above at then end of the file

* You should be now able to connect to all the VM with SSH   
  `ssh <adminUsername>@<vm_name>.azure`

## Provisioning  
* `cd provisioning`
* `ansible-playbook -i azure_hosts main.yml -u <adminUsername>`
