### Courses
https://learn.acloud.guru/course/hashicorp-certified-terraform-associate-1

# Notes
Terraform providers - https://registry.terraform.io/browse/providers

#### Terraform workflow
- terraform init
- terraform fmt
- terraform validate
- terraform plan
- terraform apply
- terraform destroy
* Skip yes while applying `terraform apply --auto-approve`
* Import to state - ```terraform import azurerm_resource_group.rg /subscriptions/0cfe2870-d256-4119-b0a3-16293ac11bdc/resourceGroups/rg1```
* If you have error with already existing resources and need to sync - ```terraform import azurerm_resource_group.rg /subscriptions/0cfe2870-d256-4119-b0a3-16293ac11bdc/resourceGroups/1-4f10a454-playground-sandbox```
Terraform State - We can also save the state remotely in S3/GCP/Blob storages
- List the state -  ```terraform state list```
- Remove state - ```terraform state rm```
- Show state - ```terraform state show```

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

# Book notes
## Terraform. Up and Running 
- https[://]digtvbg[.]com/files/LINUX/Brikman%20Y.%20Terraform.%20Up%20and%20Running.%20Writing...as%20Code%203ed%202022[.]pdf
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

### 
