# Output relevant info after deployment

### Display attributes of count objects
#### Wildcard
```terraform
output "instance_public_ips" {
  value = aws_instance.some_instances.*.public_ip
}
```

#### List comprehension
```terraform
output "instance_public_ips" {
  value = [for instance in aws_instance.some_instances : instance.public_ip]
}
```

### Display collection of values as a map
```terraform
output "some_output" {
  value = {
    some_public_ip = aws_instance.some_instance.public_ip
    some_vpc_id    = aws_vpc.some_vpc
  }
}
```