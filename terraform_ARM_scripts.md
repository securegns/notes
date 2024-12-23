3 linux VMs in a vnet in Azure, username gns, pasword Password@123
```
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {}
  skip_provider_registration = true
}

variable "resource_group_name" {
  type        = string
  default     = "1-88cca038-playground-sandbox"
  description = "The name of the existing resource group."
}

variable "vm_size" {
  type        = string
  default     = "Standard_B1s"
  description = "The size of the Virtual Machines."
}

data "azurerm_resource_group" "rg" {
  name = var.resource_group_name
}

resource "azurerm_virtual_network" "vnet" {
  name                = "myVNet"
  address_space       = ["10.0.0.0/16"]
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
}

resource "azurerm_subnet" "subnet" {
  name                 = "mySubnet"
  resource_group_name  = data.azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_security_group" "nsg" {
  name                = "myNSG"
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name

  security_rule {
    name                       = "Allow-SSH"
    priority                   = 1001
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "22"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "Allow-HTTP"
    priority                   = 1002
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "80"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }
}

resource "azurerm_subnet_network_security_group_association" "subnet_assoc" {
  subnet_id                 = azurerm_subnet.subnet.id
  network_security_group_id = azurerm_network_security_group.nsg.id
}

resource "azurerm_public_ip" "pubip" {
  count               = 3
  name                = "vm${count.index + 1}PublicIP"
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
  allocation_method   = "Static"
}

resource "azurerm_network_interface" "nic" {
  count               = 3
  name                = "vm${count.index + 1}NIC"
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.pubip[count.index].id
  }
}

resource "azurerm_linux_virtual_machine" "vm" {
  count               = 3
  name                = "myVM${count.index + 1}"
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
  network_interface_ids = [
    azurerm_network_interface.nic[count.index].id
  ]
  size                               = var.vm_size
  admin_username                     = "gns"
  admin_password                     = "Password@123"
  disable_password_authentication     = false

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-focal"
    sku       = "20_04-lts"
    version   = "latest"
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }
}

output "vm_public_ips" {
  description = "The public IP addresses of the VMs"
  value       = [for ip in azurerm_public_ip.pubip : ip.ip_address]
}

```

App gw, lin vm, WAF
<details open>
  <summary>App gw, lin vm, WAF</summary>
```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachines_myVm_name": {
            "defaultValue": "myVm",
            "type": "String"
        },
        "virtualNetworks_vnet1_name": {
            "defaultValue": "vnet1",
            "type": "String"
        },
        "publicIPAddresses_pip1_name": {
            "defaultValue": "pip1",
            "type": "String"
        },
        "networkInterfaces_vm133_name": {
            "defaultValue": "vm133",
            "type": "String"
        },
        "applicationGateways_apg1_name": {
            "defaultValue": "apg1",
            "type": "String"
        },
        "publicIPAddresses_vm1_ip_name": {
            "defaultValue": "vm1-ip",
            "type": "String"
        },
        "virtualNetworks_vm1_vnet_name": {
            "defaultValue": "vm1-vnet",
            "type": "String"
        },
        "publicIPAddresses_myVm_ip_name": {
            "defaultValue": "myVm-ip",
            "type": "String"
        },
        "networkInterfaces_myvm835_z1_name": {
            "defaultValue": "myvm835_z1",
            "type": "String"
        },
        "networkSecurityGroups_vm1_nsg_name": {
            "defaultValue": "vm1-nsg",
            "type": "String"
        },
        "networkSecurityGroups_myVm_nsg_name": {
            "defaultValue": "myVm-nsg",
            "type": "String"
        },
        "ApplicationGatewayWebApplicationFirewallPolicies_wafpol1_name": {
            "defaultValue": "wafpol1",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies",
            "apiVersion": "2024-01-01",
            "name": "[parameters('ApplicationGatewayWebApplicationFirewallPolicies_wafpol1_name')]",
            "location": "eastus",
            "properties": {
                "customRules": [
                    {
                        "name": "rule1",
                        "priority": 100,
                        "ruleType": "MatchRule",
                        "action": "Allow",
                        "matchConditions": [
                            {
                                "matchVariables": [
                                    {
                                        "variableName": "RequestUri"
                                    }
                                ],
                                "operator": "Contains",
                                "negationConditon": false,
                                "matchValues": [
                                    "id"
                                ],
                                "transforms": []
                            }
                        ],
                        "state": "Enabled"
                    }
                ],
                "policySettings": {
                    "requestBodyCheck": true,
                    "maxRequestBodySizeInKb": 128,
                    "fileUploadLimitInMb": 100,
                    "state": "Enabled",
                    "mode": "Prevention",
                    "requestBodyInspectLimitInKB": 128,
                    "fileUploadEnforcement": true,
                    "requestBodyEnforcement": true
                },
                "managedRules": {
                    "managedRuleSets": [
                        {
                            "ruleSetType": "OWASP",
                            "ruleSetVersion": "3.2",
                            "ruleGroupOverrides": []
                        }
                    ],
                    "exclusions": [
                        {
                            "matchVariable": "RequestArgNames",
                            "selectorMatchOperator": "Equals",
                            "selector": "id",
                            "exclusionManagedRuleSets": []
                        }
                    ]
                }
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkSecurityGroups_myVm_nsg_name')]",
            "location": "eastus",
            "properties": {
                "securityRules": [
                    {
                        "name": "SSH",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_myVm_nsg_name'), 'SSH')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "22",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 300,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    },
                    {
                        "name": "HTTP",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_myVm_nsg_name'), 'HTTP')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "80",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 320,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    },
                    {
                        "name": "HTTPS",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_myVm_nsg_name'), 'HTTPS')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "443",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 340,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkSecurityGroups_vm1_nsg_name')]",
            "location": "westus2",
            "properties": {
                "securityRules": [
                    {
                        "name": "SSH",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_vm1_nsg_name'), 'SSH')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "22",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 300,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    },
                    {
                        "name": "HTTP",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_vm1_nsg_name'), 'HTTP')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "80",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 320,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    },
                    {
                        "name": "HTTPS",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_vm1_nsg_name'), 'HTTPS')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "443",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 340,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_myVm_ip_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "zones": [
                "1"
            ],
            "properties": {
                "ipAddress": "172.191.169.6",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_pip1_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "zones": [
                "1",
                "2",
                "3"
            ],
            "properties": {
                "ipAddress": "74.179.210.219",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_vm1_ip_name')]",
            "location": "westus2",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "properties": {
                "ipAddress": "52.151.17.60",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2024-01-01",
            "name": "[parameters('virtualNetworks_vm1_vnet_name')]",
            "location": "westus2",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vm1_vnet_name'), 'default')]",
                        "properties": {
                            "addressPrefix": "10.0.0.0/24",
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2024-07-01",
            "name": "[parameters('virtualMachines_myVm_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_myvm835_z1_name'))]"
            ],
            "zones": [
                "1"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "Standard_D2s_v3"
                },
                "additionalCapabilities": {
                    "hibernationEnabled": false
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "canonical",
                        "offer": "ubuntu-24_04-lts",
                        "sku": "server",
                        "version": "latest"
                    },
                    "osDisk": {
                        "osType": "Linux",
                        "name": "[concat(parameters('virtualMachines_myVm_name'), '_OsDisk_1_a84762a19d8142c0984627223306d8fc')]",
                        "createOption": "FromImage",
                        "caching": "ReadWrite",
                        "managedDisk": {
                            "storageAccountType": "Premium_LRS",
                            "id": "[resourceId('Microsoft.Compute/disks', concat(parameters('virtualMachines_myVm_name'), '_OsDisk_1_a84762a19d8142c0984627223306d8fc'))]"
                        },
                        "deleteOption": "Delete",
                        "diskSizeGB": 30
                    },
                    "dataDisks": [],
                    "diskControllerType": "SCSI"
                },
                "osProfile": {
                    "computerName": "[parameters('virtualMachines_myVm_name')]",
                    "adminUsername": "gns",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": false,
                        "provisionVMAgent": true,
                        "patchSettings": {
                            "patchMode": "ImageDefault",
                            "assessmentMode": "ImageDefault"
                        }
                    },
                    "secrets": [],
                    "allowExtensionOperations": true,
                    "requireGuestProvisionSignal": true
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_myvm835_z1_name'))]",
                            "properties": {
                                "deleteOption": "Detach"
                            }
                        }
                    ]
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true
                    }
                }
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_myVm_nsg_name'), '/HTTP')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVm_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "80",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 320,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_vm1_nsg_name'), '/HTTP')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_vm1_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "80",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 320,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_myVm_nsg_name'), '/HTTPS')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVm_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "443",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 340,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_vm1_nsg_name'), '/HTTPS')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_vm1_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "443",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 340,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_myVm_nsg_name'), '/SSH')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVm_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "22",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 300,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_vm1_nsg_name'), '/SSH')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_vm1_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "22",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 300,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2024-01-01",
            "name": "[parameters('virtualNetworks_vnet1_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name'))]"
            ],
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.1.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default')]",
                        "properties": {
                            "addressPrefix": "10.1.0.0/24",
                            "applicationGatewayIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/gatewayIPConfigurations/appGatewayIpConfig')]"
                                }
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "default2",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default2')]",
                        "properties": {
                            "addressPrefix": "10.1.1.0/24",
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_vm1_vnet_name'), '/default')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_vm1_vnet_name'))]"
            ],
            "properties": {
                "addressPrefix": "10.0.0.0/24",
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_vnet1_name'), '/default2')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_vnet1_name'))]"
            ],
            "properties": {
                "addressPrefix": "10.1.1.0/24",
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_vnet1_name'), '/default')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_vnet1_name'))]",
                "[resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name'))]"
            ],
            "properties": {
                "addressPrefix": "10.1.0.0/24",
                "applicationGatewayIPConfigurations": [
                    {
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/gatewayIPConfigurations/appGatewayIpConfig')]"
                    }
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/applicationGateways",
            "apiVersion": "2024-01-01",
            "name": "[parameters('applicationGateways_apg1_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default')]",
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pip1_name'))]",
                "[resourceId('Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies', parameters('ApplicationGatewayWebApplicationFirewallPolicies_wafpol1_name'))]"
            ],
            "zones": [
                "1",
                "2",
                "3"
            ],
            "properties": {
                "sku": {
                    "name": "WAF_v2",
                    "tier": "WAF_v2",
                    "family": "Generation_1",
                    "capacity": 1
                },
                "gatewayIPConfigurations": [
                    {
                        "name": "appGatewayIpConfig",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/gatewayIPConfigurations/appGatewayIpConfig')]",
                        "properties": {
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default')]"
                            }
                        }
                    }
                ],
                "sslCertificates": [],
                "trustedRootCertificates": [],
                "trustedClientCertificates": [],
                "sslProfiles": [],
                "frontendIPConfigurations": [
                    {
                        "name": "appGwPublicFrontendIpIPv4",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/frontendIPConfigurations/appGwPublicFrontendIpIPv4')]",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pip1_name'))]"
                            }
                        }
                    }
                ],
                "frontendPorts": [
                    {
                        "name": "port_80",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/frontendPorts/port_80')]",
                        "properties": {
                            "port": 80
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "pool1",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/backendAddressPools/pool1')]",
                        "properties": {
                            "backendAddresses": []
                        }
                    }
                ],
                "loadDistributionPolicies": [],
                "backendHttpSettingsCollection": [
                    {
                        "name": "sett1",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/backendHttpSettingsCollection/sett1')]",
                        "properties": {
                            "port": 80,
                            "protocol": "Http",
                            "cookieBasedAffinity": "Disabled",
                            "pickHostNameFromBackendAddress": false,
                            "requestTimeout": 20
                        }
                    }
                ],
                "backendSettingsCollection": [],
                "httpListeners": [
                    {
                        "name": "list1",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/httpListeners/list1')]",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/frontendIPConfigurations/appGwPublicFrontendIpIPv4')]"
                            },
                            "frontendPort": {
                                "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/frontendPorts/port_80')]"
                            },
                            "protocol": "Http",
                            "hostNames": [],
                            "requireServerNameIndication": false,
                            "customErrorConfigurations": []
                        }
                    }
                ],
                "listeners": [],
                "urlPathMaps": [],
                "requestRoutingRules": [
                    {
                        "name": "route1",
                        "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/requestRoutingRules/route1')]",
                        "properties": {
                            "ruleType": "Basic",
                            "priority": 100,
                            "httpListener": {
                                "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/httpListeners/list1')]"
                            },
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/backendAddressPools/pool1')]"
                            },
                            "backendHttpSettings": {
                                "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/backendHttpSettingsCollection/sett1')]"
                            }
                        }
                    }
                ],
                "routingRules": [],
                "probes": [],
                "rewriteRuleSets": [],
                "redirectConfigurations": [],
                "privateLinkConfigurations": [],
                "enableHttp2": true,
                "firewallPolicy": {
                    "id": "[resourceId('Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies', parameters('ApplicationGatewayWebApplicationFirewallPolicies_wafpol1_name'))]"
                }
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkInterfaces_vm133_name')]",
            "location": "westus2",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_vm1_ip_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vm1_vnet_name'), 'default')]",
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_vm1_nsg_name'))]"
            ],
            "kind": "Regular",
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "id": "[concat(resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_vm133_name')), '/ipConfigurations/ipconfig1')]",
                        "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
                        "properties": {
                            "privateIPAddress": "10.0.0.4",
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_vm1_ip_name'))]",
                                "properties": {
                                    "deleteOption": "Detach"
                                }
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vm1_vnet_name'), 'default')]"
                            },
                            "primary": true,
                            "privateIPAddressVersion": "IPv4"
                        }
                    }
                ],
                "dnsSettings": {
                    "dnsServers": []
                },
                "enableAcceleratedNetworking": true,
                "enableIPForwarding": false,
                "disableTcpStateTracking": false,
                "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_vm1_nsg_name'))]"
                },
                "nicType": "Standard",
                "auxiliaryMode": "None",
                "auxiliarySku": "None"
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkInterfaces_myvm835_z1_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_myVm_ip_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default2')]",
                "[resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name'))]",
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVm_nsg_name'))]"
            ],
            "kind": "Regular",
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "id": "[concat(resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_myvm835_z1_name')), '/ipConfigurations/ipconfig1')]",
                        "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
                        "properties": {
                            "privateIPAddress": "10.1.1.4",
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_myVm_ip_name'))]"
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_vnet1_name'), 'default2')]"
                            },
                            "primary": true,
                            "privateIPAddressVersion": "IPv4",
                            "applicationGatewayBackendAddressPools": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/applicationGateways', parameters('applicationGateways_apg1_name')), '/backendAddressPools/pool1')]"
                                }
                            ]
                        }
                    }
                ],
                "dnsSettings": {
                    "dnsServers": []
                },
                "enableAcceleratedNetworking": true,
                "enableIPForwarding": false,
                "disableTcpStateTracking": false,
                "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVm_nsg_name'))]"
                },
                "nicType": "Standard",
                "auxiliaryMode": "None",
                "auxiliarySku": "None"
            }
        }
    ]
}
```
</details>
