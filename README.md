
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

**Please note**
Ansible has to be installed on ubuntu ec2 instance (AMI ID ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20220609 (ami-02f3416038bdb17fb) )
Anisble playbooks run on Ansible version 2.9.24
To install specific version of Ansible first create and install vurtual environment

* document plugin for dynamic inventory
* document installing specific ansible version in venv environment
* document iam role to connect ubuntu instance to target server
* document updating path to ssh key in playbook.yml
* document updating  subnet playbook.yml
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

# Clone this repository 

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
ssh -i ~/.ssh/my_aws.pem ec2-user@ec2-18-118-95-247.us-east-2.compute.amazonaws.com
```
![ubuntu](https://user-images.githubusercontent.com/67350852/130296102-d3a8fbfb-8b95-4d0d-9e8e-b26767bef488.png)

# Ansible Inventory File
Edit the inventory.txt file and make sure to replace the private IP with your Jenkins instance (instance created above) private IP address
```
target ansible_host=<target private ip>
```
# Running the Ansible Playbook

1. Before we run the Ansible Playbook, we need to SSH into our Jenkins Instance and accept the finger print. If we don’t do this then we will encounter errors when we try and run our Ansible Playbook. Type yes when prompted.
```
ssh -i ssh -i ~/.ssh/my_aws.pem ec2-user@ec2-18-118-95-247.us-east-2.compute.amazonaws.com
```
2. To run the Ansible Playbook targeting the Jenkins Instance run the following:
```
ansible-playbook install-jenkins.yml -i inventory.txt --private-key ~/.ssh/my_aws.pem  --ask-vault-pass 
```
Note: The yum update portion could take up to 5 minutes. 

# Post-installation Jenkins Setup

1. Navigate to http://< jenkins public ip >:8080 (replace < jenkins public ip > with your own)
  
  ![unlock_jenkins](https://user-images.githubusercontent.com/67350852/130302988-78333c4c-34f2-49a9-88f8-7177869f9af3.png)
  
2. To get the automatically-generated password SSH into your Jenkins Instance and run the following:
```
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3. Copy and paste the output to Unlock Jenkins screen and click Continue.
  
4. Click Install suggested plugins.
 
![unlock_jenkins](https://user-images.githubusercontent.com/67350852/130303122-bd084463-3565-4a18-bad7-c0cb4fa96815.png)

    
  
dynamic inventory ping command ansible aws_ec2 -i aws_ec2.yml -u ec2-user -m ping --private-key=~/.ssh/my_aws.pem  --ask-vault-pass

install packages script ansible-playbook install-jenkins.yml -i aws_ec2.yml --private-key=~/.ssh/my_aws.pem --ask-vault-pass

install chrome driver:

sudo wget https://chromedriver.storage.googleapis.com/93.0.4577.15/chromedriver_linux64.zip

sudo unzip chromedriver_linux64.zip
sudo mv chromedriver /usr/bin/chromedriver
chromedriver – version


install google chrome on amazon linux 2 :
	
sudo curl https://intoli.com/install-google-chrome.sh | bash
sudo mv /usr/bin/google-chrome-stable /usr/bin/google-chrome


google-chrome --version && which google-chrome

ansible playbook to install google chrome and chrome driver :

https://www.edureka.co/community/63433/necessary-chromedriver-selenium-chromium-selenium-ansible

