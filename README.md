# Ansible Windows Hyper-V Guest Module

An Ansible module to control Hyper-V VM's from Ansible

## Structure
- Python
- YAML
- Powershell

# Ansible Host File Description
[hyper_v_server] # Hyper-V Controller
127.0.0.1

[hyper_v_server:vars] # User with privilege to use Hyper-V Manager
ansible_user="acruz@magaya.com"
ansible_password=Magaya2024#@

[qa_machine] # Static Machine to use  with privilege to use Hyper-V Manager
192.168.55.90

[qa_machine:vars]
ansible_user="acruz@magaya.com"
ansible_password=Magaya2024#@

[virtual_clients]
192.168.9.3

[virtual_clients:vars]
ansible_user="administrator"
ansible_password=M@g@y@202$


[global_winrm_config:children]
qa_machine
hyper_v_server
virtual_clients

[global_winrm_config:vars]
ansible_connection=winrm
ansible_winrm_transport=credssp
ansible_winrm_server_cert_validation=ignore

## Notes
- This module has been tested on Ubuntu 16.04 running Ansible 2.4 (Hyper-V Server: Windows Server 2012 R2)
- Run the following script on the Windows machine in order for Ansible to be able to connect to the machine: https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

## Basic Usage
- Install Ansible 2.4
- Run `pip install pywinrm`
- Run `ConfigureRemotingForAnsible.ps1` -EnableCredSSP # Enable and secure WinRM in Window Host 
- Run `ansible-playbook -i ./src/hosts ./src/create_vm.yml --extra-vars "NID=99999"` # Manager Hyper-V VM asocied to a NID
- Run `ansible-playbook  -i src//hosts ./src/agent_install.yml --extra-vars "NID=99999"` #Reset Host Reset Agent 

