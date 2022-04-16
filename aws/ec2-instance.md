# Deploying EC2 instancees
### Create single ec2 instance
```terraform
variable "setup_id" {}

resource "aws_instance" "some_ec2_instance" {
    ami                         = data.aws_ami.ubuntu_ami.id
    instance_type               = "t3.micro"
    key_name                    = "my-ec2-key"
    user_data                   = file("${path.module}/path/to/script.sh)
    # user_data_base64            = ""
    # iam_instance_profile        = ""
    # associate_public_ip_address = true
    # availability_zone           = ""
    # source_dest_check           = false # for firewall instances
    # monitoring                  = true
    # cpu_core_count              = 2
    # cpu_threads_per_core        = 1

    network_interface {
        network_interface_id = aws_network_interface.some_nic.id
        device_index         = 0
    }

    tags = {
        Name = "${var.setup_id}-instance"
    }
}
```

### Create multiple ec2 instances of same type
```terraform
variable "num_instances" {}

resource "aws_network_interface" "some_nic" {
    subnet_id       = aws_subnet.some_subnet.id
    private_ips     = ["10.0.0.50"]
    security_groups = [aws_security_group.some_sg.id]

    tags = {
        Name = "${var.setup_id}-some_nic-${count.index}"
    }

    count = var.num_instances
}

resource "aws_instance" "some_ec2_instance" {
    ami                         = data.aws_ami.ubuntu_ami.id
    instance_type               = "t3.micro"
    key_name                    = "my-ec2-key"

    network_interface {
        network_interface_id = aws_network_interface.some_nic[count.index].id
        device_index         = 0
    }

    tags = {
        Name = "${var.setup_id}-instance-${count.index}"
    }
}

```