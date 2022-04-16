# Automating network provisioning
## VPC
### Fetching existing VPC
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/vpc

```terraform
variable "vpc_prefix" {}
variable "vpc_id" {}

data "aws_vpc" "some_vpc" {
    tags = {
        Name = "${var.vpc_prefix}-*"
    }
}

data "aws_vpc" "some_other_vpc" {
    id = var.vpc_id
}
```

### Route-tables
### Fetching existing route-table
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/route_table

```terraform
variable "subnet_id" {}
variable "rtb_id" {}

data "aws_route_table" "some_rtb" {
  subnet_id = var.subnet_id
}

data "aws_route_table" "some_other_rtb" {
  route_table_id = var.rtb_id
}
```

### Adding route to rtb
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route

```terraform
resource "aws_route" "route" {
  route_table_id              = data.aws_route_table.some_rtb.id
  destination_cidr_block      = "10.0.1.0/22"
  # vpc_peering_connection_id = ""
  # network_interface_id      = ""
  # transit_gateway_id        = ""
  # gateway_id                = ""
  # nat_gateway_id            = ""
}
```

### Create new route-table
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table

```terraform
resource "aws_route_table" "some_rtb" {
  vpc_id = aws_vpc.some_vpc.id

  route {
    cidr_block = "10.0.1.0/24"
    gateway_id = aws_internet_gateway.some_igw.id
  }

  tags = {
    Name = ""
  }
}
```

## Subnets
### Fetching existing Subnets
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/subnet

```terraform
variable "subnet_id" {}
variable "az" {}
variable "setup_id" {}

data "aws_subnet" "some_subnet" {
  id = var.subnet_id
}

data "aws_subnet" "some_other_subnet" {
    vpc_id            = data.some_vpc.id
    availability_zone = var.az

    tags = {
        Name = "${var.setup_id}-inside-*"
    }
}
```

### Creating new subnets
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet

```
variable "subnet_cidr" {}
variable "subnet_az" {}
variable "setup_id" {}

resource "aws_subnet" "some_subnet" {
    vpc_id                  = aws_vpc.main.id
    cidr_block              = var.subnet_cidr
    availability_zone       = var.subnet_az
    map_public_ip_on_launch = true

    tags = {
        Name = "${var.setup_id}-some_subnet"
    }
}
```

### Associating subnet with route-table
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table_association

```
resource "aws_route_table_association" "subnet_rtb_assoc" {
    subnet_id      = aws_subnet.some_subnet.id
    route_table_id = aws_route_table.some_rtb.id
}
```

## Network Interface
### Create
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/network_interface

```terraform
resource "aws_network_interface" "some_nic" {
    subnet_id       = aws_subnet.some_subnet.id
    private_ips     = ["10.0.0.50"]
    security_groups = [aws_security_group.some_sg.id]

    tags = {
        Name = "${var.setup_id}-some_nic"
    }
}
```

### Create and attach
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/network_interface

```terraform
resource "aws_network_interface" "some_other_nic" {
    subnet_id       = aws_subnet.some_subnet.id
    private_ips     = ["10.0.0.50"]
    security_groups = [aws_security_group.some_sg.id]

    attachment {
        instance     = aws_instance.some_intance.id
        device_index = 1
    }

    tags = {
        Name = "${var.setup_id}-some_nic"
    }
}
```