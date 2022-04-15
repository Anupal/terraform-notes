# Authentication is Terraform
### Using the tf script
- Directly mention the access and secret keys in the terraform file under "provider" block.
- The values are exposed in plaintext
- Not a good practice
    ```terraform
    provider "aws" {
        region     = "ap-south-1"
        access_key = ""
        secret_key = ""
    }
    ```
### Using AWS Cli
- download and configure awscli v2 locally in your setup
- Terrform script uses configured keys for authentication