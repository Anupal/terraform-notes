# Utilities

### Add delay before creating resources
https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/sleep

```terraform
resource "null_resource" "before" {}

resource "time_sleep" "custom_delay" {
    depends_on = [
        null_resource.before
    ]
    # sleep for 240 secs
    create_duration = "240s"
}

resource "null_resource" "after" {
    depends_on = [
        time_sleep.custom_delay
    ]
}
```

### Select local-exec script execution process based on OS
```terraform
variable "host_os" {
    type = string
    default = "windows"
}


resource "aws_instance" "some_ec2_instance" {
    ami                         = data.aws_ami.ubuntu_ami.id
    instance_type               = "t3.micro"
    key_name                    = "my-ec2-key"
    
    provisioner "local-exec" {
        command = templatefile("path"), {
            hostname     = self.public_ip,
            user         = "ubuntu",
            identityfile = "path/to/file"
        })
        interpreter = var.host_os == "windows" ? ["Powershell", "-Command"] : ["bash", "-c"]
    }
}
```