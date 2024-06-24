# certbot-lambda

Running Certbot on AWS Lambda.

Inspired by [Deploying EFF's Certbot in AWS Lambda](https://arkadiyt.com/2018/01/26/deploying-effs-certbot-in-aws-lambda/).

Forked from kingsoftgames/certbot-lambda and updated for latest versions of Python and Certbot and other packages since Python 3.8 is now deprecated on AWS

## Features

- Supports wildcard certificates (Let's Encrypt ACME v2).
- Uploads certificates to specified Amazon S3 bucket.
- Works with CloudWatch Scheduled Events for certificate renewal.
- Use Terraform to deploy to AWS (See [terraform folder](terraform)).

## How to archive zip file for lambda function
```bash
./package.sh
```

## How to deploy to S3

- In the Terrform folder, check the variables.tf file and set the variable defaults.
- Ensure the AWS environment variables are set for your AWS account.
- Enter the terraform folder and initialise terraform. It just needs an S3 bucket name to keep stuff in.
- Then apply it, it should automagically create all the necessary resources in AWS
```terraform init
terraform apply
```


## How to update certbot version

- Source virtualenv
```bash
source certbot/venv/bin/activate
```
- Recreate requirements.txt with any plugins
```bash
readonly CERTBOT_VERSION=2.10.0
readonly CERTBOT_DNS_TENCENTCLOUD_VERSION=2.0.2
pip3 install \
    certbot==${CERTBOT_VERSION} \
    certbot-dns-route53==${CERTBOT_VERSION} \
    certbot-dns-tencentcloud==${CERTBOT_DNS_TENCENTCLOUD_VERSION} # Optional dns plugin
```
- Create new requirements file
```bash
# https://stackoverflow.com/questions/39577984/what-is-pkg-resources-0-0-0-in-output-of-pip-freeze-command
pip freeze | grep -v "pkg-resources" > requirements.txt
```
