# Ansible Configuration

### Install winRM on Linux
```
sudo apt install python3-pip
pip install "pywinrm>=0.3.0"
```
### Kerberos
```
#Upgrade PIP to latest version
sudo pip3 install --upgrade pip

#Install Kerberos
sudo apt-get install python-dev libkrb5-dev krb5-user

# Install pywinrm with kerberos with pip3
pip3 install pywinrm
pip3 install pywinrm[kerberos]
```
# KerberosHost config for kerberos Authentication
```
sudo nano /etc/krb5.conf
```
Add this to [libdefaults] section
```
ticket_lifetime = 24h
renew_lifetime = 7d
rdns = false
```
Add your domain to [realms] section. Domain name in UPPERCASE!!
```
DOMMAIN_NAME.LOCAL = {
                kdc = DOMAIN_NAME.LOCAL
                default_domain = DOMAIN_NAME.LOCAL
        }
```
Add your domain to [domain_realm] section. Domain name in UPPERCASE!! 
```
.domain_name.local = DOMAIN_NAME.LOCAL
domain_name.local = DOMAIN_NAME.LOCAL
```
Init your user ticket
```
kinit username@MY.DOMAIN.COM
```
### Ansible.cfg
```
sudo nano /etc/ansible/ansible.cfg
```
Add path to invertory folder
```
[defaults]
inventory = $HOME/hosts
```
Create HOSTS file in your $Home dir
```
cd $Home
touch hosts
```
Open Hosts file and add next strings

### Hosts file 
```
[webservers]
host1.domain_name.local
host2.domain_name.local

[webservers:vars]
ansible_user=*LOGIN* #Domain must be written in UPPERCASE
ansible_password=*PASSWORD*
ansible_connection=winrm
ansible_ssh_port=5986
ansible_winrm_server_cert_validation=ignore
ansible_winrm_port=5985
ansible_python_interpreter= C:\Users\admin\AppData\Local\Programs\Python\Python39-32
ansible_winrm_transport=ntlm
#ansible_become=false
#ansible_become_method: runas
```
### Ping all Windows Clients
```
ansible all -m win_ping
```
