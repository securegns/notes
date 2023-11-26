# Course notes
A cloud could also have ...
- One account which can manage multiple accounts (ex. AWS orginisations)
- Cross account access (ex. Azure guest users)
- In GCP by default role attached to compute instance, in AWS/Azure its not enabled by default

### Apps URL
AWS  -  orgname.awsapps.com/start
Microsoft - https://myapps.microsoft.com/
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
