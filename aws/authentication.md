# Authentication in Terraform
### In provider block
#### Keys in plaintext
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
#### Keys as environmental variables
- Terraform automatically picks up following environment variables
    ```bash
    export AWS_ACCESS_KEY_ID="someaccesskey"
    export AWS_SECRET_ACCESS_KEY="somesecretkey"
    export AWS_DEFAULT_REGION="someregion"
    ```
#### Keys in AWS Profile
- Keys can be stored in `~/aws/credentials` file as follows:
    ```
    [someawsprofile]
    aws_access_key_id     = someaccesskey
    aws_secret_access_key = somesecretkey
    ```
- Export this profile as an environment variable `export AWS_PROFILE=someawsprofile`
- Reference the profile in provider block
    ```terraform
    provider "aws" {
        profile = "someawsprofile"
        region  = "ap-south-1"
    }
    ```

### Using AWS Cli
- download and configure awscli v2 locally in your setup
- Terrform script uses configured keys for authentication