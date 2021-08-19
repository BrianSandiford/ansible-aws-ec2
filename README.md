# Using Ansible to provision AWS EC2 instances.

Simple approach to use Ansible for provisioning an AWS EC2 instance.

The following steps will be performed:

* Create AWS user
* Install Ansible and Ansible EC2 module dependencies
* Create SSH keys
* Create Ansible structure
* Run Ansible to provision the EC2 instance
* Connect to the EC2 instance via SSH

ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2

ansible-playbook install-jenkins.yml -i inventory.txt --private-key ~/.ssh/my_aws.pem  --ask-vault-pass 
