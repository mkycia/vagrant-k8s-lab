## Create k8s lab with vagrant VMs and ansible
----
* supports Hyper-V or Virtualbox or libvirt (kvm)
* support multiple VMs in one Vagrantfile
* uses ansible_local provisioner to install and manage k8s stuff. Actually supports:
  * CentOS 7 (or any other RHEL based distro)
  * docker-ce
  * minikube
  * kind
  * k8s tools:
    * buildpacks
    * skaffold
    * octant


### To provision on hyper-v:
> `vagrant up --provider hyperv`

### To provision on virtualbox
> `vagrant up --provider virtualbox`

### To provision on libvirt (kvm) | assuming that vagrant plugin is installed on linux host with libvirt (kvm)
> `vagrant up --provider libvirt`

### Hyper-v NAT network config (vagrant provider for Hyper-v doesn't support it like virtualbox, so it has to be done manually)



```
PS C:\> New-VMSwitch -SwitchName NAT -SwitchType Internal
PS C:\> Get-NetAdapter

Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
----                      --------------------                    ------- ------       ----------             ---------
(...)
vEthernet (NAT)           Hyper-V Virtual Ethernet Adapter #4         118 Up           00-00-00-00-00-11        10 Gbps
(...)


PS C:\> New-NetIPAddress -IPAddress 192.168.200.1 -PrefixLength 24 -InterfaceIndex 118
PS C:\> New-NetNat -Name NAT -InternalIPInterfaceAddressPrefix "192.168.200.0/24"
```

## Usage
----

* To configure VMs amount and parameters edit config.yml
* To define which k8s stuff to install edit ansible/playbook.yml
