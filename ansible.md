### inventory file
```
target1 ansible_host=10.10.10.10 ansible_ssh_pass=Password123
```

### Check connection
```ansible target1 -m ping -i inventory.txt```

### set host key checking false
- File: ```/etc/ansible/ansible.cfg```
- Code: ```host_key_checking=False```
