### Courses
https://learn.acloud.guru/course/hashicorp-certified-terraform-associate-1

### Notes
Terraform providers - https://registry.terraform.io/browse/providers

#### Terraform workflow
- terraform init
- terraform fmt
- terraform validate
- terraform plan
- terraform apply
- terraform destroy

* Import to state - ```terraform import azurerm_resource_group.rg /subscriptions/0cfe2870-d256-4119-b0a3-16293ac11bdc/resourceGroups/rg1```
* List the state -  ```terraform state list```
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

* 

