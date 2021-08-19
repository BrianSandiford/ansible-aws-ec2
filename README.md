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
Open the AWS [Console](https://us-east-2.console.aws.amazon.com/console/home?region=us-east-2), search for IAM (Identity and Access Management) and follow this steps to create a user and take note of the Access Key and Secret Key that will be used by Ansible to set up the instances.

ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2

ansible-playbook install-jenkins.yml -i inventory.txt --private-key ~/.ssh/my_aws.pem  --ask-vault-pass 
