### inventory file
```
target1 ansible_host=10.10.10.10 ansible_ssh_pass=Password123
```
Other params
- ansible_connection=ssh/winrm/localhost
- ansible_port=22/5986
- ansible_user=gns
- ansible_password=password123
By default ansible_user is set to root

### Check connection
```ansible target1 -m ping -i inventory.txt```

### set host key checking false
- File: ```/etc/ansible/ansible.cfg```
- Code: ```host_key_checking=False```
