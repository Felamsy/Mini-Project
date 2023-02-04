# ALTSCHOOL AFRICA

## How To Create a Load Balancer, Instances, and A Hosted Domain Name In Route 53 Using Terraform

#### Before we start,we need to set up some stuffs first

1. Create IAM user & access keys

2. Store access key for use

3. Install terraform

## Creating An IAM User With An Access Key And Secret Access Key For Terraform Access

### CREATING AN IAM USER ON AWS

1. Log in to the AWS Management Console.
2. Go to the IAM service.
3. Click on the "Users" section on the left-side panel.
4. Click on the "Add user" button.
5. Enter a name for the user.
6. Choose the necessary permissions for the user.
7. Click on the "Next: Review" button
8. Review the user details and click on the "Create user" button.
9. Save the Access key ID and Secret access key for the user.
   **Image Guide Below**

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user%201.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user2.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20USer3.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user4.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user5.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user%206.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20user7.jpg)

![AWS Iam USER](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20Iam%20User%208.jpg)

### ADDING ACCESS AND SECRET ACCESS KEY TO AWS CLI

In this section we are going to be adding the access key and secret access key to the AWS CLI

1. Open the terminal or command prompt

2. Login to your linux server

3. Run this command below

sudo apt update && sudo apt dist-upgrade -y

sudo apt install awscli

4. After running these commands in your terminal you can run the AWS CLI to be sure that it is installed.

5. Configuring Our Aws Cli
   Type "aws configure" and press enter.

Enter the Access key ID and press enter.

Enter the Secret access key and press enter.

Enter the Secret access key and press enter.

Choose the default region name or enter a custom one.

Choose the default output format or enter a custom one(JSON Recommended)

The Access key ID and Secret access key are now set up in AWS CLI and you can use them to run AWS commands.

**Image Guide Below**

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/CLI%201.jpg)

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/CLI%202.jpg)

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20CLI%201.jpg)

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20CLI%202.jpg)

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20CLI%203.jpg)

![AWS CLI](https://github.com/Felamsy/Mini-Project/blob/main/image/AWS%20CLI%204.jpg)

### INSTALLING TERRAFORM

To install terraform you visit this site: https://developer.hashicorp.com/terraform/downloads

You can see in the operating system the different types you can install on. we are going with Linux and Ubuntu/Debian.

1. wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

2. $ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

3. $ sudo apt update && sudo apt install terraform

above are the commands you will copy one by one and run on your ubuntu machine.

terraform -v

the above is the command to check if terraform is installed

**Image Guide Below**

![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/Terraform%201.jpg)

![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/Installing%20terraform%201.jpg)

![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/Installing%20terraform%202.jpg)

### creating a terraform variable file

To create a Terraform variable file, follow these steps:

1. Create a new file with a ".tfvars" extension (e.g. variables.tfvars).

2. Add the necessary variables to the file in the format "variable_name = value". For example:

access_key = "ACCESS_KEY_HERE"
secret_key = "SECRET_KEY_HERE"
region = "us-west-2"

**Image example below**

![TERRAFORM variable](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20variable.jpg)

NOTE: if you are storing it in a variable file you must not push the variable file to your git hub because it’s not safe.

### CREATING A VPC WITH ITS DEPENDENCIES ON TERRAFORM

Firstly you need create a folder or directory and call it terraform. in that folder create a file and name it main.tf.

To create a Virtual Private Cloud (VPC) with its dependencies on Terraform, you need to write Terraform configuration files to define the resources you want to create. Here is an example of how you could create a VPC with public and private subnets:

terraform {
required_providers {
aws = {
source = "hashicorp/aws"
}
}
}
![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/Main%201.jpg)

##### Create VPC

resource "aws_vpc" "Altschool_vpc" {
cidr_block = "10.0.0.0/16"
enable_dns_hostnames = true
tags = {
Name = "Altschool_vpc"
}
}

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%202.jpg)

in the main.tf file you input this script inside.

#### Internet Gateway

To create an Internet Gateway for your VPC, you can add another resource to your Terraform configuration:

##### Create Internet Gateway

resource "aws_internet_gateway" "Altschool_internet_gateway" {
vpc_id = aws_vpc.Altschool_vpc.id
tags = {
Name = "Altschool_internet_gateway"
}
}

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%203.jpg)

you can name it whatever you want to name it. but make sure that the vpc id is being placed in this block.

#### Route Tables

Route tables in Amazon Web Services (AWS) VPCs are used to control the traffic routing between subnets in a VPC. Each subnet in a VPC must be associated with a route table, which controls the traffic routing for the subnet.

##### Create public Route Table

resource "aws_route_table" "Altschool-route-table-public" {
vpc_id = aws_vpc.Altschool_vpc.id

route {
cidr_block = "0.0.0.0/0"
gateway_id = aws_internet_gateway.Altschool_internet_gateway.id
}

tags = {
Name = "Altschool-route-table-public"
}

copy the script above and also add it to the main.tf file you created.

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%204.jpg)

#### Subnet Route Table

To associate a subnet with a specific route table in AWS VPC, you can use the aws_route_table_association resource in Terraform.

this script is going to be used by terraform to create our public route table for the two subnets that we are going to be creating

##### Associate public subnet 1 with public route table

resource "aws_route_table_association" "Altschool-public-subnet1-association" {
subnet_id = aws_subnet.Altschool-public-subnet1.id
route_table_id = aws_route_table.Altschool-route-table-public.id
}

##### Associate public subnet 2 with public route table

resource "aws_route_table_association" "Altschool-public-subnet2-association" {
subnet_id = aws_subnet.Altschool-public-subnet2.id
route_table_id = aws_route_table.Altschool-route-table-public.id

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%205.jpg)

#### public Subnet

A public subnet in AWS VPC is a subnet that is connected to the Internet through an Internet Gateway. Instances in a public subnet can communicate with the Internet, while instances in a private subnet cannot communicate with the Internet directly

##### Create Public Subnet-1

resource "aws_subnet" "Altschool-public-subnet1" {
vpc_id = aws_vpc.Altschool_vpc.id
cidr_block = "10.0.1.0/24"
map_public_ip_on_launch = true
availability_zone = "us-east-1a"
tags = {
Name = "Altschool-public-subnet1"
}
}

##### Create Public Subnet-2

resource "aws_subnet" "Altschool-public-subnet2" {
vpc_id = aws_vpc.Altschool_vpc.id
cidr_block = "10.0.2.0/24"
map_public_ip_on_launch = true
availability_zone = "us-east-1b"
tags = {
Name = "Altschool-public-subnet2"
}
}

in our subnets, we are adding different availability zone for the two different subnets.

one is in EU-west-2a and the other is in EU-west-2b. they should not be in the same availability zones.

because our load balancer will be using these two subnets

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%206.jpg)

#### Network Acl

A network ACL (access control list) is a security mechanism for controlling traffic in and out of a subnet in a Virtual Private Cloud (VPC) in cloud computing. It acts as a firewall at the subnet level, allowing or denying traffic based on the rules specified in the ACL. Network ACLs have separate inbound and outbound rule sets and support allowing or denying traffic based on IP address, port number, and protocol. Network ACLs are stateless, meaning that return traffic must be explicitly allowed by rules in the ACL

##### Create a network ACL

resource "aws_network_acl" "Altschool-network_acl" {
vpc_id = aws_vpc.Altschool_vpc.id
subnet_ids = [aws_subnet.Altschool-public-subnet1.id, aws_subnet.Altschool-public-subnet2.id]

ingress {
rule_no = 100
protocol = "-1"
action = "allow"
cidr_block = "0.0.0.0/0"
from_port = 0
to_port = 0
}

egress {
rule_no = 100
protocol = "-1"
action = "allow"
cidr_block = "0.0.0.0/0"
from_port = 0
to_port = 0
}
}

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%207.jpg)

and as you can see here we added our vpc id and our two subnets id.

NOTE: everything in the vpc section must be in the main.tf file

### CREATING SECURITY GROUP

To create a security group using Terraform, you can define the desired security group rules in Terraform configuration file and use the aws_security_group resource type to manage the security group. Here is an example of creating a security group that allows incoming SSH traffic:

we are going to create security groups for two things.

1. our load balancer.

2. our Ec2 instances.

The ingress in terraform basically means inbound rules and the egress means outbound rule.

# Create a security group for the load balancer

resource "aws_security_group" "Altschool-load_balancer_sg" {
name = "Altschool-load-balancer-sg"
description = "Security group for the load balancer"
vpc_id = aws_vpc.Altschool_vpc.id

ingress {
from_port = 80
to_port = 80
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]
}

ingress {
from_port = 443
to_port = 443
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]

}

egress {
from_port = 80
to_port = 80
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]
}
}
copy the above code and also put it in the main.tf file

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%209.jpg)

#### Ec2 Instance Security Group

In Terraform, an EC2 instance security group is defined using the "aws_security_group" resource. This resource creates a security group that you can use to control inbound and outbound network traffic to your EC2 instance.

we will need to create our Ec2 instance security group to be able to SSH into the instances.

##### Create Security Group to allow port 22, 80 and 443

resource "aws_security_group" "Altschool-security-grp-rule" {
name = "allow_ssh_http_https"
description = "Allow SSH, HTTP and HTTPS inbound traffic for public instances"
vpc_id = aws_vpc.Altschool_vpc.id

ingress {
description = "HTTP"
from_port = 80
to_port = 80
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]
security_groups = [aws_security_group.Altschool-load_balancer_sg.id]
}

ingress {
description = "HTTPS"
from_port = 443
to_port = 443
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]
security_groups = [aws_security_group.Altschool-load_balancer_sg.id]
}

ingress {
description = "SSH"
from_port = 22
to_port = 22
protocol = "tcp"
cidr_blocks = ["0.0.0.0/0"]

}

egress {
from_port = 0
to_port = 0
protocol = "-1"
cidr_blocks = ["0.0.0.0/0"]

}

tags = {
Name = "Altschool-security-grp-rule"
}
}

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2010.jpg)

all this should also be in the main.tf file

### CREATING INSTANCES

In Terraform, you can create EC2 instances using the "aws_instance" resource

# creating instance 1

resource "aws_instance" "Altschool1" {
ami = "ami-00874d747dde814fa"
instance_type = "t2.micro"
key_name = "holidayProject"
security_groups = [aws_security_group.Altschool-security-grp-rule.id]
subnet_id = aws_subnet.Altschool-public-subnet1.id
availability_zone = "us-east-1a"

tags = {
Name = "Altschool-1"
source = "terraform"
}
}

##### creating instance 2

resource "aws_instance" "Altschool2" {
ami = "ami-00874d747dde814fa"
instance_type = "t2.micro"
key_name = "holidayProject"
security_groups = [aws_security_group.Altschool-security-grp-rule.id]
subnet_id = aws_subnet.Altschool-public-subnet2.id
availability_zone = "us-east-1b"

tags = {
Name = "Altschool-2"
source = "terraform"
}
}

##### creating instance 2

resource "aws_instance" "Altschool2" {
ami = "ami-00874d747dde814fa"
instance_type = "t2.micro"
key_name = "holidayProject"
security_groups = [aws_security_group.Altschool-security-grp-rule.id]
subnet_id = aws_subnet.Altschool-public-subnet2.id
availability_zone = "us-east-1b"

tags = {
Name = "Altschool-2"
source = "terraform"
}
}

##### creating instance 3

resource "aws_instance" "Altschool3" {
ami = "ami-00874d747dde814fa"
instance_type = "t2.micro"
key_name = "holidayProject"
security_groups = [aws_security_group.Altschool-security-grp-rule.id]
subnet_id = aws_subnet.Altschool-public-subnet1.id
availability_zone = "us-east-1a"

tags = {
Name = "Altschool-3"
source = "terraform"
}
}

This is how we are going to create our three instances. if you look. the ami (amazon machine image) that we are using is ubuntu.

The instance type is t2.micro. to stay in the range of our free tier. and also if you look at the security groups we added the one we create for the instance.

Now notice that this subnet for instances 1 and 3 are the same but instance 2 is in another subnet

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2012.jpg)
![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2013.jpg)
![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2014.jpg)

### Storing Ip address

In Terraform, you can store the IP addresses of your EC2 instances using output variables. Output variables allow you to expose values from your Terraform configuration to be easily queried and reused elsewhere.

Now there is a very important thing here the file name.

When creating a file using the local_file module. the file name must be the exact path you want terraform to create the file

Example:-(/home/vagrant/Terraform/host-inventory)

#### Create a file to store the IP addresses of the instances

resource "local_file" "Ip_address" {
filename = "/home/vagrant/Terraform/host-inventory"
content = <<EOT
${aws_instance.Altschool1.public_ip}
${aws_instance.Altschool2.public_ip}
${aws_instance.Altschool3.public_ip}
EOT
}

/_ resource "local_file" "Ip_address" {
filename = "/home/vagrant/Terraform/host-inventory"
content = <<EOT
%{for ip_addr in aws_instance.Altschool._.public_ip~}
${ip_addr}
%{endfor~}
EOT
}
\*/

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%208.jpg)

### CREATING AN APPLICATION LOAD BALANCER

In Terraform, you can create an Application Load Balancer (ALB) using the "aws_alb" resource. An ALB is a type of load balancer that operates at the application layer (layer 7) and is used to route traffic to multiple targets, such as EC2 instances.

we are creating an application load balancer in this section to be able to switch between our instances

#### Create an Application Load Balancer

resource "aws_lb" "Altschool-load-balancer" {
name = "Altschool-load-balancer"
internal = false
load_balancer_type = "application"
security_groups = [aws_security_group.Altschool-load_balancer_sg.id]
subnets = [aws_subnet.Altschool-public-subnet1.id, aws_subnet.Altschool-public-subnet2.id]
#enable_cross_zone_load_balancing = true
enable_deletion_protection = false
depends_on = [aws_instance.Altschool1, aws_instance.Altschool2, aws_instance.Altschool3]
}

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2016.jpg)

in our load balancer. we made the type application and in our subnets, we added our public subnet 1 and subnet 2

we also added our three instances to the load balancer. if you also noticed our security group is that of the load balancer

### Creating Target Groups

In Terraform, you can create an ALB Target Group using the aws_alb_target_group resource. A target group is used to route traffic to one or more registered targets, such as EC2 instances, containers, or IP addresses.

#### Create the target group

resource "aws_lb_target_group" "Altschool-target-group" {
name = "Altschool-target-group"
target_type = "instance"
port = 80
protocol = "HTTP"
vpc_id = aws_vpc.Altschool_vpc.id

health_check {
path = "/"
protocol = "HTTP"
matcher = "200"
interval = 15
timeout = 3
healthy_threshold = 3
unhealthy_threshold = 3
}
}

we just added our vpc id and changed the target type to the instance with a protocol HTTP. the health check can be left as the default like above

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2017.jpg)

### Listener & Listener Rule

In Terraform, you can create an Application Load Balancer Listener and Listener Rule using the aws_alb_listener and aws_alb_listener_rule resources, respectively.

A Listener is associated with a Load Balancer and specifies the front-end connection settings, such as port and protocol, for incoming requests. A Listener Rule is used to route incoming requests to one or more target groups based on the content of the request, such as the URL path or host header.

we are going to be adding a listener rule to our load balancer.

A listener is a process that checks for connection requests, using the protocol and port that you configure.

The rules that you define for a listener determine how the load balancer routes request to the targets in one or more target groups.

# Create the listener

resource "aws_lb_listener" "Altschool-listener" {
load_balancer_arn = aws_lb.Altschool-load-balancer.arn
port = "80"
protocol = "HTTP"

default_action {
type = "forward"
target_group_arn = aws_lb_target_group.Altschool-target-group.arn
}
}

#### Create the listener rule

resource "aws_lb_listener_rule" "Altschool-listener-rule" {
listener_arn = aws_lb_listener.Altschool-listener.arn
priority = 1

action {
type = "forward"
target_group_arn = aws_lb_target_group.Altschool-target-group.arn
}

condition {
path_pattern {
values = ["/"]
}
}
}

in the above , we set our target group arn to our load balancer target group arn that we created before.

we also made the default action and action type to forward.

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2018.jpg)

### Attaching The Target Groups To The Load Balancer

In Terraform, you can attach a target group to an Application Load Balancer by using the aws_alb_target_group_attachment resource.

#### Attach the target group to the load balancer

resource "aws_lb_target_group_attachment" "Altschool-target-group-attachment1" {
target_group_arn = aws_lb_target_group.Altschool-target-group.arn
target_id = aws_instance.Altschool1.id
port = 80

}

resource "aws_lb_target_group_attachment" "Altschool-target-group-attachment2" {
target_group_arn = aws_lb_target_group.Altschool-target-group.arn
target_id = aws_instance.Altschool2.id
port = 80
}

resource "aws_lb_target_group_attachment" "Altschool-target-group-attachment3" {
target_group_arn = aws_lb_target_group.Altschool-target-group.arn
target_id = aws_instance.Altschool3.id
port = 80

}

here we are just adding the load balancer target group arn to the target group arn.

and we are also adding the instances id.

all on port 80

![MAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/main%2011.jpg)

### CREATING THE ROUTE 53

In Terraform, you can create a Route 53 resource record set using the aws_route53_record resource

in this section, we are going to be writing a terraform script that helps us set up a hosted zone on route 53.

we are doing this because just using our load balancer DNS is not really practical.

now create a new file in that same directory. name it route53.tf

variable "domain_name" {
default = "felixolusegun.me"
type = string
description = "Domain name"
}

#### get hosted zone details

resource "aws_route53_zone" "hosted_zone" {
name = var.domain_name

tags = {
Environment = "dev"
}
}

#### create a record set in route 53

#### terraform aws route 53 record

resource "aws_route53_record" "site_domain" {
zone_id = aws_route53_zone.hosted_zone.zone_id
name = "terraform-test.${var.domain_name}"
type = "A"

alias {
name = aws_lb.Altschool-load-balancer.dns_name
zone_id = aws_lb.Altschool-load-balancer.zone_id
evaluate_target_health = true
}

in the file, there are three sections as you can see. first, we are creating a variable where we will store our domain name in the default.

for me, it is felixolusegun.me and for you, it should be different. in the type, we set it as a string, and the description is domain name.

second, we are creating a hosted zone where we called our variable in the name section.

and lastly, we are creating an A record with the name starting with terraform-test. and the variable of the domain name listed above follows it

we also added our zone id from the zone we created above it.

in the alias section, we are adding the name of our load balancer through its DNS name.

and also the zone id.

![ROUTE 53](https://github.com/Felamsy/Mini-Project/blob/main/image/route53%201.jpg)

### Outputs

create a last file and name it outputs.tf. in this file we are going to be printing out the output of our load balancer.

// load balancer outputs

output "elb_target_group_arn" {
value = aws_lb_target_group.Altschool-target-group.arn
}

output "elb_load_balancer_dns_name" {
value = aws_lb.Altschool-load-balancer.dns_name
}

output "elastic_load_balancer_zone_id" {
value = aws_lb.Altschool-load-balancer.zone_id
}

where we can easily collect the DNS name to check if everything is working fine after running ansible.

![OUTPUTS](https://github.com/Felamsy/Mini-Project/blob/main/image/lboutput.jpg)

NOTE: when you are done with the terraform script first thing to write is terraform init

to initialize terraform. simple. and it’s a must.

second thing: terraform plan

this helps you see the plan of the script.

the last thing: terraform apply

this is to execute everything in the terraform script.

![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20innit.jpg)
![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20plan%201.jpg)
![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20plann%202.jpg)
![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20apply1.jpg)
![TERRAFORM](https://github.com/Felamsy/Mini-Project/blob/main/image/terraform%20apply%202.jpg)

### THE ANSIBLE PLAYBOOK

in this section we are going to be configuring ansible to do three things:

1 install apache

2 print our IP address

3 change our instances timezone to Lagos/Africa

you can change the time zone to whatever country you are in but as for me, that’s mine.

![ANSIBLE](https://github.com/Felamsy/Mini-Project/blob/main/image/ansible%201.jpg)

---

- hosts: all
  become: true
  tasks:

  - name: update and upgrade the servers
    apt:
    update_cache: yes
    upgrade: yes

  - name: install apache2
    tags: apache, apache2, ubuntu
    apt:
    name: - apache2
    state: latest

  - name: set timezone to Africa/Lagos
    tags: time
    timezone: name=Africa/Lagos

  - name: print hostname on server
    tags: printf
    shell: echo "<h1>This is my server name $(hostname -f)</h1>" > /var/www/html/index.html

  - name: restart apache2
    tags: restart
    service:
    name: apache2
    state: restarted

The ansible file above will help us do everything that is to be done even updating and upgrading the servers.

#### Ansible Config File

in the ansible config file, you will set up where your private key is.

[defaults]
inventory = host-inventory
remote_user = ubuntu
private_key_file = /home/vagrant/Terraform/holidayProject.pem

![ANSIBLE CONFIG](https://github.com/Felamsy/Mini-Project/blob/main/image/ansible%20config.jpg)

like the one above. the private key file is in the path I specified above.

and the inventory file was the name of the file I created with terraform where all my IP addresses are stored by terraform.

we will be needing a remote user because while running your playbook ansible may throw you an error.

that you should use the remote ubuntu user and not root.

ansible-playbook -i host-inventory site.yml

the above command is to run the playbook

![ANSIBLE](https://github.com/Felamsy/Mini-Project/blob/main/image/ansible%20result%201.jpg)

![ANSIBLE](https://github.com/Felamsy/Mini-Project/blob/main/image/ansible%20result%202.jpg)

NOTE: make sure your .pem file is in the same directory where you are running the ansible-playbook

and ssh into each server before running the ansible playbook. if not it will hang.

### PUTTING YOUR ROUTE 53 NAME SERVERS IN THE DOMAIN PROVIDER.

we are going to be putting our name servers that were created in our route 53 hosted zone in the domain provider.

in my case I used namecheap.com you might have used something else but the method is going to be the same.

![ROUTE 53](https://github.com/Felamsy/Mini-Project/blob/main/image/route%2053%20dashboard.jpg)

now on my route 53 dashboard, you can see that terraform already created a hosted zone for my domain name. so now we click on the hosted zone.

![HOSTED ZONE](https://github.com/Felamsy/Mini-Project/blob/main/image/hosted%20zone.jpg)

now that you are in your hosted zone you click on your hosted zone name.

![HOSTED ZONE](https://github.com/Felamsy/Mini-Project/blob/main/image/hosted%20zone2.jpg)

we are now in. above you will see three record names. we will be ticking on the first one. the one with the name servers and type NS.

and on it, we will be copying the 4 name servers that route 53 created for us.

we are going to be pasting it into our domain provider.

### Putting Our Name Servers On Our Domain Provider.

we will now head over to namecheap.com like I said before you can use any domain provider of your choice the process is the same

![NAMECHEAP](https://github.com/Felamsy/Mini-Project/blob/main/image/namecheap%20domain.jpg)

now that we are here I will be clicking on the domain name so that I can access and manage it.

![NAMECHEAP](https://github.com/Felamsy/Mini-Project/blob/main/image/nameservers.jpg)

and now we go back to route 53 and copy the name servers one by one and paste them here.

now that we are done adding the 4 name servers we are good to go.

from here we just wait for our route 53 to come up.

![DOMAIN](https://github.com/Felamsy/Mini-Project/blob/main/image/domain%20result.jpg)

HERE WE GO !!!
