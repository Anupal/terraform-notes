# EIPs
### Fetching existing EIP
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eip

```terraform
data "aws_eip" "by_allocation_id" {
    # id      = "eipalloc-12345678" # search by allocation id
    public_ip = "1.2.3.4"

    tags = {
        Name = "some_eip_name"
    }
}
```

### Create new EIP
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip

#### Create
```terraform
resource "aws_eip" "some_eip_name" {
    tags = {
        Name = "some_eip_name"
    }
}
```

#### Create and attach to instance
```terraform
resource "aws_eip" "some_other_eip_name" {
    instance = aws_instance.some_instance.id
    vpc      = true
}
```

#### Create and attach to NIC
```terraform
resource "aws_eip" "some_other_eip_name" {
    network_interface         = aws_network_interface.some_nic.id
    associate_with_private_ip = "10.0.0.10
    vpc      = true
}
```