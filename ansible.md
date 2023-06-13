### inventory file
```
target1 ansible_host=10.10.10.10 ansible_ssh_pass=Password123
```

### Check connection
```ansible target1 -m ping -i inventory.txt```

### set host key checking false
```/etc/ansible/ansible.cfg```
```host_key_checking=False```
