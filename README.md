# Using Ansible to provision AWS EC2 instances.

Simple approach to use Ansible for provisioning an AWS EC2 instance.

The following steps will be performed:

* Create AWS user
* Install Ansible and Ansible EC2 module dependencies
* Create SSH keys
* Create Ansible structure
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

ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2

ansible-playbook install-jenkins.yml -i inventory.txt --private-key ~/.ssh/my_aws.pem  --ask-vault-pass 
