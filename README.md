# Ansible-vSphere-Bulk-Import

This Ansible playbook provides an easy method to create multiple VMs from templates in VMware vSphere. Parameters could be supplied via CSV file or default_ variables. To run the playbook, you have to install Ansible and additional modules:
```
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible python3-pip -y
pip install pyvmomi
pip install netaddr
```
