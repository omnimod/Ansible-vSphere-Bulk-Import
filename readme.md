# Ansible-vSphere-Bulk-Import

## Overview
This Ansible playbook provides an easy method to create multiple VMs from templates in VMware vSphere. It's based on [vmware_guest](https://docs.ansible.com/ansible/latest/collections/community/vmware/vmware_guest_module.html#ansible-collections-community-vmware-vmware-guest-module) module.

 Parameters could be supplied via CSV file or default_ variables.
 
## Installation and usage
To run this playbook tou have to install Ansible and additional modules. For example in Ubuntu Linux 22.04 run:
```
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible python3-pip -y
pip install pyvmomi
pip install netaddr
```

Download project files from GitHub:
```
git clone https://github.com/omnimod/Ansible-vSphere-Bulk-Import.git
```

Copy vars-deploy-vms.yaml.example file to vars-deploy-vms.yaml and edit it to update default values for your environment. For additional information on default values look for [Default values](#default-values) paragraph:
```
cp vars-deploy-vms.yaml.example vars-deploy-vms.yaml
```

Copy vms.csv.example to vms.csv and edit it to specify VMs parameters. For additional information about .csv file look for [CSV file](#csv-file) paragraph:
```
cp vms.csv.example vms.csv
```

To create VMs run the playbook:
```
ansible-playbook deploy-vsphere-vms.yaml
```

## Default values
vars-deploy-vms.yaml contains predefined variables and default values which can be used to create new VMs. If you don't supply parameters in CSV file, default values from vars-deploy-vms.yaml or VM template will be used during VMs creation. The list of accepted predefined variables and default values:
- vsphere_url - FQDN or IP address of vCenter Server to connect to.
- vsphere_login - login to connect to vCenter Server (administrator@vsphere.local or something else).
- vsphere_password - password to connect to vCenter Server.
- validate_certs - set True if certificate validation is needed.
- debug - set True if you want to get additional information during playbook execution for debugging purposes.
- input_file - path to the CSV file.
- def_vm_template - default VM template to create new VMs for (this and other def_ values will be override by values from CSV file for specific VM).
- def_vm_cluster - vSphere cluster where you want to place new VMs.
- def_vm_resource_pool - vSphere resource pool where you want to place new VMs.
- def_vm_folder - vSphere folder where you want to place new VMs.
- def_vm_datacenter - vSphere datacenter where you want to place new VMs.
- def_disk_type - disk provisioned type: thin, thick, or eagerzeroedthick.
- def_disk_mode - disk mode: persistent, independent_persistent, or independent_nonpersistent.
- def_datastore - datastore to place VMs disks.
- def_network_netmask - netmastk.
- def_network_gateway - gateway.
- def_network_domain - domain name.
- def_vm_password - guest OS default account password (root or administrator).
- def_vm_state - VM state after creation: poweredon or poweredoff.
- def_wait_for_ip_address - wait for the current VM fully boot and gets IP address before creating next one.
- def_network_dns_servers - list of DNS servers.

## CSV file
vms.csv is a comma-separated file which containes input parameters for created VMs. The list of accepted parameters/columns:
- name - VM name. This parameter is required.
- cluster - vSphere cluster where you want to place new VMs.
- domainadmin - login to join VM to domain.
- domainadminpassword - password to join VM to domain.
- dns_servers - list of DNS servers.
- dns_suffix - DNS suffix to search.
- joindomain - Join VM to domain: true of false.
- orgname - company name.
- password - guest OS default account password (root or administrator).
- timezone - time zone.
- datacenter - vSphere datacenter where you want to place new VMs.
- folder - vSphere folder where you want to place new VMs.
- memory_mb - amount of RAM in MB for VM.
- num_cpus - number of CPUs for VM.
- num_cpu_cores_per_socket - number of cores per socket. For example if you specified num_cpus = 8 and num_cpu_cores_per_socket = 4, VM will have two sockets with 4 cores in each.
- secure_boot - enable secure boot: true or false.
- resource_pool - vSphere resource pool where you want to place new VMs.
- state - VM state after creation: poweredon or poweredoff.
- template - template to create VM from.
- wait_for_ip_address - wait for the current VM fully boot and gets IP address before creating next one.
- disk{n}.size_gb - size of disk{n} in GB, where n - is the number of disk, for exaple disk0.size_gb, disk1.size_gb, disk2.size_gb, etc. You can create VM with multiple virtual disks. For example setting disk0.size_gb = 40, disk1.size_gb = 20, disk2.size_gb = 1 will create a VM with three disks of 40 GB, 20 GB, and 1 GB respectively. You have to define disk{n}.size_gb parameter for each virtual disk in VM template.
- disk{n}.type - disk n provisioned type: thin, thick, or eagerzeroedthick.
- disk{n}.disk_mode - disk n mode: persistent, independent_persistent, or independent_nonpersistent.
- disk{n}.datastore - datastore to place VMs disk n.
- network{n}.name - portgroup name to connect virtual nic to, where n - is the number of virtual nic, for example network0.name, network1.name, network2.name, etc. You can create VM with multiple virtual nics. For example setting network0.name = "VM Network", network1.name = "VM Network 2", network2.name = "VM Network 3" will create a VM with three nics and connect them to networks "VM Network", "VM Network 2", "VM Network 3" respectively. You have to define network{n}.name parameter for each virtual nic in VM template.
- network{n}.ip - set static IP for nic n.
- network{n}.netmask - set netmask for nic n.
- network{n}.gateway - set default gateway for nic n.
- network{n}.dns_servers - set DNS servers for nic n. You can specify multiple DNS servers separated by comma, for example: "192.168.1.2, 192.168.1.3". If you do that, don't forget to use double quotes to escape comma character.
- network{n}.domain - set domain name for nic n.

You can skip some parameters in CSV file or completly remove the whole column. In this case playbook will use default values from vars-deploy-vms.yaml file.