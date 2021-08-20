
# Using Ansible to provision AWS EC2 instances.

Simple approach to use Ansible for provisioning an AWS EC2 instance.This README was adapted from a longer tutorial which you can view [here](https://medium.datadriveninvestor.com/devops-using-ansible-to-provision-aws-ec2-instances-3d70a1cb155f)

The following steps will be performed:

* Create AWS user
* Install Ansible and Ansible EC2 module dependencies
* Create SSH keys
* Run Ansible to provision the EC2 instance
* Connect to the EC2 instance via SSH

# Create AWS user
Open the AWS [Console](https://us-east-2.console.aws.amazon.com/console/home?region=us-east-2), search for IAM (Identity and Access Management) and follow these  [steps](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console) to create a user and take note of the Access Key and Secret Key that will be used by Ansible to set up the instances.

# Install Ansible and the EC2 module dependencies

Instructions on installing the latest version of ansible can be found [here](https://www.cyberciti.biz/faq/how-to-install-and-configure-latest-version-of-ansible-on-ubuntu-linux/)

Install pip and boto3
``` 
sudo apt install python-pip
pip install boto boto3 ansible
```

Ansible version 2.9.24 and Python version 2.7.17 used

# Create SSH keys to connect to the EC2 instance after provisioning
```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my_aws
```
Rename my_aws as my_aws.pem 

# Create Ansible Vault file to store the AWS Access and Secret keys.
```
ansible-vault create group_vars/all/pass.yml
New Vault password:
Confirm New Vault password:
```

The password provided here will be asked every time the playbook is executed or when editing the pass.yml file.

# Edit the pass.yml file and create the keys global constants

Create the variables ec2_access_key and ec2_secret_key and set the values gathered after user creation (IAM).
```
ansible-vault edit group_vars/all/pass.yml 
Vault password:
ec2_access_key: AAAAAAAAAAAAAABBBBBBBBBBBB                                      
ec2_secret_key: afjdfadgf$fgajk5ragesfjgjsfdbtirhf
```

## Directory structure
```
➜  AWS_Ansible tree
.
├── group_vars
│   └── all
│       └── pass.yml
└── playbook.yml
2 directories, 2 files
```

# Running Ansible to provision instances
## Create the instance
```
ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2
```

![creat_instance](https://user-images.githubusercontent.com/67350852/130295118-27c5039a-59a3-4040-ac0f-0bf05fb5a9c8.png)

For security, the playbook will execute by default just the tasks to collect information on AWS. The tasks responsible for provisioning the instance will be performed if the tag create_ec2 is specified.

## Get the public DNS
```
ansible-playbook playbook.yml --ask-vault-pass
```
![pubic_dns](https://user-images.githubusercontent.com/67350852/130295538-23899bc3-9154-429b-9023-1d428d49946a.png)

# Connect to the EC2 instance via SSH
```
ssh -i ssh -i ~/.ssh/my_aws.pem ec2-user@ec2-18-118-95-247.us-east-2.compute.amazonaws.com
```
![ubuntu](https://user-images.githubusercontent.com/67350852/130296102-d3a8fbfb-8b95-4d0d-9e8e-b26767bef488.png)


ansible-playbook install-jenkins.yml -i inventory.txt --private-key ~/.ssh/my_aws.pem  --ask-vault-pass 



