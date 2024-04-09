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

### Notes
- VM Service accounts/Instance Metadata Service (IMDS) - EC2 - Instance profile, Azure - Managed identity, GCP - Service account
##### EC2
- Get security group ```curl –s "http://169.254.169.254/latest/meta-data/securitygroups/"```
-  
