# Pointcloud Tiler Cloud Deployment

## AWS

### Requirements

* Packer
* Terraform
* Ansible
* Python
* aws CLI

### Install Packer Ansible Terraform

Packer:

[Installation Guide Packer](https://learn.hashicorp.com/tutorials/packer/getting-started-install#precompiled-binaries)

Terraform:

[Installation Guide Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

Ansible:
```
python -m -venv venv
source venv/bin/activate
pip install ansible
```

### Setup AWS CLI
To configure your aws cli run

``` 
aws configure 
```

Afterwards insert your credentials. This should look like:

```
$ aws configure
AWS Access Key ID [None]:  <Your AWS ACCESS KEY>    
AWS Secret Access Key [None]: <Your AWS Secret Access Key>
Default region name [us-west-2]: <Region you want operate in>
Default output format [json]: <Output format of CLI>
```

create a new role in ~/.aws/config 

[profile produser]
role_arn = arn:aws:iam::<Your Role ID>:role/oe240/FraunhoferUserRole
source_profile = default
region = us-west-2
output = json

### Build the AWS AMI

Run packer with a Makefile command

```
make packer-build
``` 

### Init Terraform

Run terraform plan to validate the script and see the changes terraform intends to perform

```
make terraform-init
``` 

### Apply Terraform

Run terraform apply to setup the infrastructure with AWS, this command is going to prompt you. Type ```yes``` to actually deploy the VMs
```
make terraform-apply
```

### Using preconfigured SSH via Hostfile template

To use the generated ssh-config file you have to either 
1) use the connect make command 
2) move the config file to the specific location ```~/.ssh/config```

Connect to the Pointcloud-Tiler Instance:

```
make connect
```

or use the following command to move the file and **overwrite** the old ssh-config:

```
move-ssh-config-to-ssh-directory
```

### Provision the running ec2 instance

```
make provisioning
```

for a more verbose provisioning use:

```
make provisioning-verbose
```

### Additional Information

The .pem file needs to have a specific permission. AWS is going to complain if the file doesn't meet the required permission.
```
chmod 400 <name of keypair pem file>.pem
```
