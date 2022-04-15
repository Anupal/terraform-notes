# Fetching AMIs
- Create a data source for the AMI
- Reference the AMI ID when creating instances

### Fetching AWS AMI
```terraform
data "aws_ami" "aws_ami" {
    most_recent = true
    owners = ["amazon"]

    filter {
        name   = "architecture"
        values = ["x86_64"]
    }

    filter {
        name   = "virtualization-type"
        values = ["hvm"]
    }

    filter {
        name   = "root-device-type"
        values = ["ebs"]
    }

    filter {
        name   = "name"
        values = "amzn2-ami-hvm*"]
    }
}
```

### Fetching Ubuntu AMI
```terraform
data "aws_ami" "ubuntu_ami" {
    most_recent = true
    owners = ["099720109447"]

    filter {
        name   = "architecture"
        values = ["x86_64"]
    }

    filter {
        name   = "virtualization-type"
        values = ["hvm"]
    }

    filter {
        name   = "root-device-type"
        values = ["ebs"]
    }

    filter {
        name   = "name"
        values = "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
    }
}
```

### Fetching RHEL AMI
```terraform
data "aws_ami" "rhel_ami" {
    most_recent = true
    owners      = [309956199498"]

    filter {
        name   = "architecture"
        values = ["x86_64"]
    }

    filter {
        name   = "virtualization-type"
        values = ["hvm"]
    }

    filter {
        name   = "root-device-type"
        values = ["ebs"]
    }

    filter {
        name   = "name"
        values = "RHEL-8.5*"]
    }
}
```