### Courses
https://learn.acloud.guru/course/hashicorp-certified-terraform-associate-1

# Notes
Terraform providers - https://registry.terraform.io/browse/providers
Security Terraform State in Azure - https://techcommunity.microsoft.com/t5/fasttrack-for-azure/securing-terraform-state-in-azure/ba-p/3787254

#### Terraform workflow
- terraform init
- terraform fmt
- terraform validate
- terraform plan
- terraform workspace show
- terraform apply
- terraform destroy
- terraform graph - to show dependency graph
* Skip yes while applying `terraform apply --auto-approve`
* Import to state - ```terraform import azurerm_resource_group.rg /subscriptions/0cfe2870-d256-4119-b0a3-16293ac11bdc/resourceGroups/rg1```
* If you have error with already existing resources and need to sync - ```terraform import azurerm_resource_group.rg /subscriptions/0cfe2870-d256-4119-b0a3-16293ac11bdc/resourceGroups/1-4f10a454-playground-sandbox```
Terraform State - We can also save the state remotely in S3/GCP/Blob storages
- List the state -  ```terraform state list```
- Remove state - ```terraform state rm```
- Show state - ```terraform state show```
- Delete only 1 resource ```terraform destroy -target azurerm_redis_cache.myrediscache```

Skip provider reg if we SPN dont have enough privs to do so
```
provider "azurerm" {
        features {}
        skip_provider_registration = "true"
}
```
Vars
```
variable "myvar1"{
        discription = "New var 1"
        type = string
        default = "Hello world"
}
```
```
variable "myvar1" {}
```
local vars 
```
locals {
  resource_group_name        = "us-west-2"
}

rg = local.resource_group_name
```
* Refrence a var `var.myvar1`
* Vars can have validation condiations

```
variable "myvar1"{
        discription = "New var 1"
        type = string
        validation {
                condiation = lenght(var.myvar1) > 4
                error_message = "Str must be grather than 4 chars"
        }
}
```
* Sensitive vars can be used for secrets
```
variable "myvar1"{
        discription = "New var 1"
        type = string
        default = "Hello"
        sensitive = true
}
```

var types
- Base types - Str, Num, Bool
- Complex types - List, Set, Map, Tuple, Object

Terraform output
```
output "instance_ip"{
        discription = "VM private IP"
        value = aws_instance.my-vm.private_ip
}
```
```terraform output public_ip```
Save the session in the S3 bucket
```
terraform {
 backend "s3" {
 # Replace this with your bucket name!
 bucket = "terraform-up-and-running-state"
 key = "global/s3/terraform.tfstate"
 region = "us-east-2"
 # Replace this with your DynamoDB table name!
 dynamodb_table = "terraform-up-and-running-locks"
 encrypt = true
 }
}
```
# Book notes
## Terraform. Up and Running 
- https[://]digtvbg[.]com/files/LINUX/Brikman%20Y.%20Terraform.%20Up%20and%20Running.%20Writing...as%20Code%203ed%202022.pdf
- Github : https://github.com/brikis98/terraform-up-and-running-code/tree/master/code
### Interesting Questions
- Why use IaC at all?
- What are the differences between configuration management, orchestration, provisioning, and server templating?
- When should you use Terraform, Chef, Ansible, Puppet, Pulumi, CloudFormation, Docker, Packer, or Kubernetes?
- How does Terraform work, and how do you use it to manage your infrastructure?
- How do you create reusable Terraform modules?
- How do you securely manage secrets when working with Terraform?
- How do you use Terraform with multiple regions, accounts, and clouds?
- How do you write Terraform code that’s reliable enough for production usage?
- How do you test your Terraform code?
- How do you make Terraform a part of your automated deployment process?
- What are the best practices for using Terraform as a team?

### Important points
- Terraform helpful for creating/maintaining reusable infrastructure, and also helpful for state configuration of cloud
- There are five broad categories of IaC tools:
        * Ad hoc scripts - ex. Powershell, Bash scripts that automate setup
        * Configuration management tools - ex. Ansible, Chef
        * Server templating tools - ex. Docker, Packer, Vagrant
        * Orchestration tools - ex. K8s 
        * Provisioning tools - ex. Terraform, Cloudformation
- Dont forget the Don’t Repeat Yourself (DRY) principle
- Advantages of IaC - Less members can manage infra, Speed and safety, Documentation, Version control, Validation, Reuse,
- IAC tools - Configuration Management Versus Provisioning, Mutable Infrastructure Versus Immutable Infrastructure, Procedural Language Versus Declarative Language, General-Purpose Language Versus Domain-Specific Language, Master Versus Masterless, Agent vs Agentless
- Procedural code does not fully capture the state of the infrastructure
- Procedural code limits reusability
- (+) is to create, (-) is to delete, (~) is to modify a resource
- Add ```.terraform *.tfstate *.tfstate.backup``` to .gitignore file
- user_data can be used to run commands on a vm while creation
- user_data_replace_on_change set to true means, if user_data has a change in tf script, the vm gets re-created
- Terraform graph output is graph description language called DOT which can be made into graphical image using tools
- A variable has - description, default, type
- Type constraints supported - string, number, bool, list, map, set, object, tuple, any
- If you don’t specify a type, Terraform assumes the type is any.
- ```validation``` - Can define a custom validation rules
- ```sensitive``` - Terraform will not log those vars, helpful for passwords/keys
- Pass a variable                -         terraform plan -var "server_port=8080"
- Set variable as env var        -         export TF_VAR_server_port=8080
- If we do not have a default value set to var, it will prompt user to provide value while applying
- In terraform interpolation is possible with ```${...}```
- If variable has sensitive set to true, the output var also need to have sensitive set to true to indicate we are intentionally outputing a secret
- ```depends_on``` useful for depends on another resource managed by terraform
- We can see outputs with ```terraform output public_ip```
- Add lifecycle lifecycle ```create_before_destroy = true``` to create update or delete
- If you set create_before_destroy to true, Terraform will invert the order in which it replaces resources, creating the replacement resource first
- Challenges with state file - Shared storage for state files, Locking state files, Isolating state files
- We can apply lock ```-lock-timeout=10m``` will wait for 10 minutes
- Partial configurations ```terraform init -backend-config=backend.hcl```
- Terraform workspaces
- If you use Terragrunt, you can run commands across multiple folders concurrently using the run-all command.
-  
### Personal challenges faced
- Forgetting to update terraform version in the main file
- Upon initially creating the VM, its IP address does not appear in the output variables. However, after running `terraform apply` a second time, the IP address becomes visible in the output variables.
- I am not able to find a way to put exceptions for any resource
- If I have a script with n number of resources and I need to deploy only a VM in azure, I need to deploy all the dependency resources including VM
- In Azure, the process of creating a resource through Terraform can be inconsistent because the required parameters for the resource block may vary between needing a name or an ID. This inconsistency can be frustrating since Terraform itself doesn't enforce a uniform method, with the variation instead stemming from the specific requirements of the cloud provider's API.
- Sometimes we add some data part(JSON?XML etc), and then perform terraform apply, says nothing changed.
