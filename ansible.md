### Notes
SSH install and enable for windows
```
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Start the sshd service
Start-Service sshd

# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'

# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
We require 2 files, called `inventory` which has info about target machine, and playbook yml file

### Running playbook
```ansible-playbook -i inventory playbook.yaml```

### Inventory file
Naming convention for this file is `hosts` or `inventory`
```
[windows]
winvm ansible_host=10.01.01.01 ansible_user=gns ansible_password=Password@123 ansible_connection=ssh
```
Other params
- ansible_connection=ssh(or)winrm(or)localhost
- ansible_port=22(or)5986
- ansible_user=gns
- ansible_password=password123
By default ansible_user is set to root

Run using this command:
```
ansible-playbook -i inventory create_user.yml

```

We can group servers into groups and also groups of groups
```
[web_servers]
web1

[db_servers]
db1

[all_servers:children]
web_servers
db_servers
```

### Check connection
```ansible target1 -m ping -i inventory.txt```

### set host key checking false
- File: ```/etc/ansible/ansible.cfg```
- Code: ```host_key_checking=False```

### Vars
```
vars:
  dns_server: 10.10.10.10
tasks:
  - linefile
       path: /etc/hosts
       line: 'dnsserver {{ dns_server }}'
```
```
# sample vars file
http_port: 8080
service: httpd
```
### Loops
```
- name: 'Install required packages'
  hosts: localhost
  become: yes
  vars:
    packages:
      - httpd
      - make
      - vim
  tasks:
    - yum:
        name: '{{ item }}'
        state: present
      with_items: '{{ packages }}'
```
### Roles
```ansible-galaxy init mysql```
```ansible-galaxy search mysql```


### Important terms
- idempotency
