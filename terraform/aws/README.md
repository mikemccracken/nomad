# Provision a Nomad cluster on AWS

## Pre-requisites

To get started, create the following:

- AWS account
- [API access keys](http://aws.amazon.com/developers/access-keys/)
- [SSH key pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)

## Set the AWS environment variables

```bash
$ export AWS_ACCESS_KEY_ID=[AWS_ACCESS_KEY_ID]
$ export AWS_SECRET_ACCESS_KEY=[AWS_SECRET_ACCESS_KEY]
```

## Build an AWS machine image with Packer

[Packer](https://www.packer.io/intro/index.html) is HashiCorp's open source tool 
for creating identical machine images for multiple platforms from a single 
source configuration. The Terraform templates included in this repo reference a 
publicly avaialble Amazon machine image (AMI) by default. The AMI can be customized 
through modifications to the [build configuration script](../shared/scripts/setup.sh) 
and [packer.json](packer.json).

Use the following command to build the AMI:

```bash
$ packer build packer.json
```

## Provision a cluster with Terraform

`cd` to an environment subdirectory:

```bash
$ cd env/us-east
```

Update `terraform.tfvars` with your SSH key name and your AMI ID if you created 
a custom AMI:

```bash
region                  = "us-east-1"
ami                     = "ami-d42d74ae"
instance_type           = "t2.medium"
key_name                = "KEY_NAME"
server_count            = "3"
client_count            = "4"
```

You can also modify the `region`, `instance_type`, `server_count`, and `client_count`. 
At least one client and one server are required.

Provision the cluster:

```bash
$ terraform init
$ terraform get
$ terraform plan
$ terraform apply
```

## Access the cluster

SSH to one of the servers using its public IP:

```bash
$ ssh -i /path/to/private/key ubuntu@PUBLIC_IP
```

The infrastructure that is provisioned for this test environment is configured to 
allow all traffic over port 22. This is obviously not recommended for production 
deployments.

## Next Steps

Click [here](../README.md#test) for next steps.