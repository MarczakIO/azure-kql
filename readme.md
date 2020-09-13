# Resource Graph Examples

## NSG open ports
Find all Network Security Groups (NSG) which has inbound rules for RDP (3389) and SSH (22)

```kql
resources 
| where type startswith 'microsoft.network/networksecuritygroups' 
| mv-expand rules=properties.securityRules
| where rules.properties.destinationPortRange in ("3389", "22")
| project 
    name    = rules.name, 
    access  = rules.properties.access, 
    rule    = rules.properties.destinationPortRange, 
    nsgname = name, 
    group   = resourceGroup
```

### Output
```
name access rule nsgname       group    
---- ------ ---- -------       -----    
RDP  Allow  3389 amdemo-vm-nsg az-900-vm
SSH  Allow  22   amdemo-vm-nsg az-900-vm
```