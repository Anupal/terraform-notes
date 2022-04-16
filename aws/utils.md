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