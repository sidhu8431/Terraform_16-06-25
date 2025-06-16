# Terraform_16-06-25


session -22
-------------------------------------------------------------------------------------------------------------------------------------------------------------

Terraform --> IaaC
----------------
EC2
R53
IAM users

Manual Infra
-----------------
Everything in console.... by mistake if someone edit wrong, then app will go down...30min-1hr
application restore back to previous stage if something goes wrong

Version control

Consistent Infra --> All environment configs and infra should be some..

CRUD --> Create infra, read, update, deleting the infra

Inventory/resource management --> If you see tf script, you know what are the services you are using

Cost optimisation --> creation in 5min, deletion in 5min

Dependency management --> sg, ec2 instance after creation of all dependencies

Code reuse --> Roles. Modules

Declarative way of creating infra --> You are giving orders to terraform to create infra just by providing right syntax

easy syntax
no sequence
state management --> Terraform can track what it created, can update easily

mysql --> mysqlll


HCL --> Hashicorp configuration language

Download terraform
keep terraform.exe in some folder
edit the environment variables, provide the path

aws configure --> AWS command line install

Resources

Terraform --> AWS, AZure, GCP, Alibaba, Digital Ocean, etc. GitHub, Networking, etc. These are the providers
----------
variables
data types
conditions
loops
functions
	data sources
	locals
	outputs
	providers
	provisoners
	
Create EC2 instance through terraform
-------------------------------------
terraform file extension is .tf

provider.tf --> where you can declare what provider you are using

resource "resource-type" "name-of-resource" {
	key = value
}

name
description
ingress mandatory
egress mandatory

ingress --> incoming traffic
egress --> outgoing traffic

while entering cabin you have to scan your ID...while exit the cabin just push the switch

terraform init --> intialise terraform, it will connect with provider and downloads it
keep .gitignore always

terraform plan --> cant create resources. it will just plan

terraform apply --> 

terraform apply -auto-approve


name
ami
sg-id
instance_type
key_pair

ansible playbook.yaml = ansible -v playbook.yaml

ansible -vv playbook.yaml
ansible -vvv playbook.yaml


----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -23
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Variables
Conditions
Data types
loops
functions

Variables
------------
x=1, y=2

variable is container that holds value...

PERSON=Ramesh

vars:
	PERSON: Ramesh
	
variable "person"{
	default = "Ramesh"
	type = string
}

string name = "ramesh"

string
number
list
map
boolean

[ --> list
{ --> map

Tagging Strategy
----------------------
Project
Component/Module
Environment

Expense
---------
MySQL
Backend
Frontend

Environment
-----------
DEV
PROD

terraform.tfvars
--------------------
using this file, we can override the default values in variables or else you can set the values also.

terraform.tfvars and default values

1. command line
2. terraform.tfvars
3. environment variables
4. default

conditions
--------------------

if (expression){
	run this if expression is true
}
else{
	run this if expression if false
}

expression ? "run this if true" : "run this if false"

environment is prod t3.small or t3.micro

outputs
-------------------
every resource exports some values, we can take them and create other resources

loops
--------------------
1. count based loop
2. for or for each

count.index --> 0
count.index --> 1
count.index --> 2

functions
--------------------
Terraform has no custom functions, We must use in-built functions

merge --> merges 2 lists

list-1 --> name=siva, course=devops
list-2 --> name=siva, course=terraform, duration=120hr
list-3 --> name=kumar, course=aws
merge(list-1,list-2)

name=siva, course=terraform
name=kumar, course=aws, duration=120

3 ec2 instances
r53 records

mysql.daws81s.online --> pvt ip
backend.daws81s.online --> pvt ip
daws81s.online --> public ip

without .gitigore

git add git commit git push

500Mb file push
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -24
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Top to Bottom Approach
----------------------
1. What is the problem? --> Manual infra
2. How terraform is solving? --> Through script, Infra as a Code
3. Apply

Resources, Providers

1. faster releases
2. less defects

1. AWS Resource/Service How it works
2. It needs some input/arguments
3. Providers will give us outputs

ps -ef | grep ssh

4. Use those outputs and create other resources

variables, data types, conditions, loops and functions

1. 3 ec2 instances
2. 3 r53 records

created resources, get the outputs and create other resources

AMI ID frequently changes...whenever you update something in AMI, ID will changes

You can query existing info from the providers. this is possible data sources.

Backend API --> creating records(POST), getting data(GET)

search the product, apply the filters --> this is query

devops-practice, rhel-9, apply other filters too


data "aws_ami" "joindevops" {

	most_recent      = true
	owners = ["973714476881"]
	
	filter {
		name   = "name"
		values = ["RHEL-9-DevOps-Practice"]
	}
	
	filter {
		name   = "root-device-type"
		values = ["ebs"]
	}

   filter {
	name   = "virtualization-type"
	values = ["hvm"]
   }
}

all recent AMI from joindevops with name RHEL-9-DevOps-Practice

inputs --> args to create resources
outputs --> after creation of resource. Public IP, private IP, instance ID, etc.
data sources  --> instead of getting args manually, you can query existing information

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session - 25
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

1. 3 ec2
2. 3 r53 records
	backend.daws81s.online --> t3.micro
	mysql.daws81s.online --> t3.small
	daws81s.online --> t3.micro
	
expression ? "true" : "false"

r53
-----
we should get the output of ec2 instanced created
aws_instance.terraform

backend.daws81s.online
var.instance_names = backend
domain_name = daws81s.online
"${var.instance_names[count.index]}.${var.domain_name}"

locals
---------------------
locals are like variables but it have some extra capabilities. You can store expressions and intermediate values in locals

variables can be overriden
but we can't override locals

1. variables and locals both can store values, but locals have some extra capabilities
2. locals can store expressions, terraform can run them and get the value
3. locals can use variables inside. variables can't refer locals
4. can override variables, can't override locals

state, remote state and locking
---------------------
mysql --> mysqllll

it is a simple name update, but ansible created another instance

assignment
---------------
I will check your notes and confirm whether you completed or not

declared infra, actual infra

terraform == declarative way of creating infra

tf files == infra I declared
aws infra == actual infra created

declared infra == actual infra

terraform.tfstate == terraform keeps track of what it created

aws_security_group.allow_ssh_terraform: Refreshing state... [id=sg-0994f93b69e9e3736]

before I delete
----------------
declared infra == actual infra --> true

After I delete
----------------
declared infra == actual infra --> false

terraform.tfstate --> instance created
real infra --> no, actually destroyed
config files --> create the infra


terraform.tfstate will be refereshed against real infra

remote state
----------------
I am creating infra from my laptop --> it will create infra
another person also tries to crate --> it will also create infra again

duplicates or errors

.lock --> programs will check if any .lock file is there, it will not allow others to edit 

remote storage --> s3 bucket
locking --> dynamo DB --> LockID

81s-locking
81s-remote-state

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -26
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

Terraform
------------
IaaC advantages
variables
data types
conditions
loops --> count based loop, count.index --> used for lists
functions

locals --> to store expressions
outputs --> provide the output of resources, like IP, id, etc.
data sources --> query the information from provider like AMI ID, etc.
state and remote state --> terraform uses state concept to compare declared vs actual/real infra. We keep state remotely in colloboration envrionment.
locking --> make sure infra provisioning is not running parellely
tfvars --> to override default variables

for each is used to iterate map...

expense infra
-------------
frontend -> t3.micro
backend --> t3.micro
mysql --> t3.small
[] --> list
{} --> map

if frontend name should be daws81s.online otherwise backend/mysql.daws81s.online

dynamic
--------

provisioners
------------------
provisioners are used to take some actions locally or remotely..

local --> where terraform executed is local...my laptop
remote --> inside the servers you created..inside the servers of backend, frontend, mysql. etc.

1. local-exec
2. remote-exec

remote-exec --> execute commands inside remote server


Module development
--------------------

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -27
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Conistent infra across all env
------------------------------
1. tfvars
2. terraform workspaces
3. seperate repos

resource defnition

left side --> arguements
right side --> values

tfvars --> override the default variables

dev.tfvars --> this should be for dev env
prod.tfvars --> this should be for prod env

backend -->s3 and dynamo db

1. keep the same bucket but diff key
2. keep diff buckets for diff env and diff key

expense infra
---------------
3 ec2, 1 sg, 3 r53 records

mysql-dev
backend-dev
frontend-dev

mysql-dev.daws81s.online
backend-dev.daws81s.online
frontend-dev.daws81s.online


mysql-prod
backend-prod
frontend-prod

mysql-prod.daws81s.online
backend-prod.daws81s.online
daws81s.online

workspaces
------------
ec2 instance
if dev t3.micro
if prod t3.medium

terraform.workspace == prod
terraform.workspace == dev


advantages
-------------
1. code reuse --> same code

disadvantage
-------------
1. easy to do errors
2. not easy to implement
3. changes made can effect all environments


seperate code for seperate env
-------------------------

expense-infra-dev

expense-infra-prod

disadvantage
--------------
duplicated code

Module development
------------------------
DRY

variables
functions
roles

functions --> inputs and outputs, we call function. you can call infinite times

write code once and call them many times...

modules -> resource defnition and arguements are same. only values are different
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -28
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
advantages
---------------
code reuse
updates are easy and centralised
best practices can be enforced
you can restrict user using few options as per project standards


VPC --> Virtual private cloud
-----------------------
business is restaurant orders

physical space
buy servers
power connection
network connection
security gaurd
cooling
OS resources, network resources
maintance

Cloud resource with cloud account

It is a isolated datacenter in cloud. resources created inside vpc is completely private...

frontend server --> public must access this
backend server --> secure, public should not access this. dont create public ip and no internet
mysql server --> all users and orders data, cards, etc...dont create public ip and no internet

You have to seperate servers logically inside VPC...

subnetting

village name, pincode --> VPC name, CIDR
streets name, number --> subnets name, CIDR
roads --> routes
main road --> internet connection, internet gateway
main gate of house --> security group/firewall
house --> server

CIDR --> classless interdomain routing

192.178.3.4 --> 4 octates

255.255.255.255 --> Max IP

10.0.0.0/16 --> CIDR

total IP address bits are 32. possible IP address are 2^32

10.0.0.1
10.0.0.2
.
.
.
10.0.0.255

10.0.1.0
10.0.1.1
.
.
.
10.0.1.255

2^16 = 65,536

10.0.0.0/24 --> first 3 are blocked

10.0.0.255

10.0.1.0/24 --> 10.0.1.0 ... 10.0.1.255
10.0.2.0/24 --> 10.0.2.0, 10.0.2.1 .... 10.0.2.255

10.0.1.0/32

VPC creation
subnet creation
igw creation

public and private subnet
-------------------------
subnet which has internet connection is called as public subnet. private/app subnet will not have internet connection as routes. database subnet is also called as private subnet

10.0.0.0/16 --> internal roads

create vpc
create igw and associate with VPC
create public, private and database subnets
create public, private and database route table
create routes inside route table
associate route tables with appropriate subnets
created elastic IP
created NAT gateway
added NAT routes in private and database subnets

secure servers can't be reached directly...this is incoming/ingress traffic
traffic from the servers ... outgoing/egress traffic

database --> yum install mysql-server --> outgoing

what is NAT --> this is the mechanism private servers connects to internet for outgoing traffic like packages installation, security patches downloads

NAT --> when server stops and starts IP address will change.It should have same IP always

static IP/ elastic IP

High availability
-------------------
HYD --> Region
east hyderabad --> AZ
west hyderabad --> AZ

1 public subnet in us-east-1a, 1 public subnet in us-east-1b
1 private subnet in us-east-1a, 1 private subnet in us-east-1b
1 database subnet in us-east-1a, 1 database subnet in us-east-1b


EC2 Module
------------
it should accept count/instance_names
use instance_names inside tags

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -29
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
- Terraform is an IAC (Infrastructure as Code) tool
- Automate, reuse, version controlling
- Hashicorp, hcl (Hashicorp language)
- Declarative
- What ever we give, it will create

- resources
- provider
- provisioners
- functions
- variables
- local-exec
- remote-exec
- data sources
- state and remote state
- locking
- modules
- loops
- workspaces
- tfvars
- conditions
- locals

- What is the difference between normal variable declaration and locals?
A) We can write expressions and evaluate them using locals

- Can you use count-based loop inside locals?
A) No

- terraform init
  - -reconfigure
  - -backend-config
  - -upgrade
- terraform validate: It perform syntax check and validates our code
- terraform plan
  - -var-file
- terraform apply
- teraform fmt
- terraform destroy
- terraform state show
- terraform workspace

- -upgrade: It is usually used to upgrade the latest version of the module source code

- terraform taint: Using this command, you could taint a terraform resource
- modules: 
  - Root module: terraform-aws-ec2
  - Child: ec2-module-demo
  
- terraform state:
  - Desired state: What you desire
  - Current state: What is your current infra that terraform is managing

- How to handle a situation when a state file is deleted or corrupted?
A) We need to import the resources that are part of the terraform code into its state file and we do it using: terraform import

terraform import aws_instance.web i-0bec8c7d30e5ab951

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -30
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Create VPC -> 10.0.0.0/16 --> 2^16 IP address
Create igw
associate igw to vpc
create subnets --> Public, private and DB
EIP
NAT
created route tables and added routes
	Public --> Internet connection through IGW
	Private --> NAT, egress connections
route table associations with subnets


terraform naming resources
-----------------------------
1. terraform resource name --> use _, no upper case
2. dont repeat resource type in name
3. if only one resource of its type, name it as main or this
4. Use - inside arguments values and in places where value will be exposed to a human
5. use plural if multiple resources

https://www.terraform-best-practices.com/naming

common tags --> common for all resources under this project
resource tags --> vpc_tags

vpc --> expense-dev

HA --> atleast 2 AZ

public --> 1a, 1b --> 10.0.1.0/24, 10.0.2.0/24
private --> 1a, 1b --> 10.0.11.0/24, 10.0.12.0/24
database --> 1a,1b --> 10.0.21.0/24, 10.0.22.0/24

1. get the AZ
2. get first 2

0,1 --> only 0th element
0,2 --> 0th element, 1st element

expense-dev-public-us-east-1a

we need to create database_subnet_group --> all database subnets under a group

1. command line
2. tfvars
3. default

4. 
----------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -31
----------------------------------------------------------------------------------------------------------------------------------------------------------------

vpc
igw
public private database subnets in 1a and 1b AZ
eip
natgateway

route tables
routes
associations with subnets

expense-dev-public

associations
--------------
1 public rt --> 2 public subnets

Peering
-------------

dev vpc prod vpc
by default vpc are not connected with each other

VPC peering can establish between two VPC.
VPC CIDR should be different, they should not overlap...

VPC-1 --> 10.0.1.123
10.0.1.122 --> 10.0.1.123

VPC-2 --> 10.0.1.123

same account
---------------
same region and diff VPC can peer
diff region and diff VPC can peer

2 accounts
---------------
diff account same region diff VPC can peer
diff account diff region diff VPC can peer


Peering
----------------
ask user whether he wants VPC peering or not. if he say yes our module will connect with default vpc in the same region

persons = ["ramesh","suresh","raheem","robert"]

persons[1]

persons = ["john"]

persons[0]

public servers
backend servers
database servers

---------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -32
---------------------------------------------------------------------------------------------------------------------------------------------------------------
mysql
backend
frontend

expense-dev-mysql

expense-vpc
expense-sg
expense-mysql

/roboshop/prod/vpc_id
/roboshop/dev/vpc_id


your-repo
--------------------
module "your_name" {
	args-as-per-module-definiton = your-value
	enable_dns_hostnames = var.dns_hostnames
}

variables.tf
------------
variable "dns_hostnames"{
	default = false
}

module.your_name.<output-parameter-name>

module == function == inputs --> outputs

10.0.0.0/16

10.1.0.0/16

1. custom modules
2. open source modules

-------------------------------------------------------------------------------------------------------------------------------------------------------
Session -33
--------------------------------------------------------------------------------------------------------------------------------------------------------

Mysql --> backend

Mysql
-------
3306
Port no: 3306
IP: backend private IP
private IP will not change after restart, public ip will change after restart
private IP may not same after termination and creation..


MYSQL SG will allow Backend SG
3306
Port no: 3306
Source: Backend SG --> MySQL will allow connection from instances which are attached to Backend SG

backend --> frontend
8080
source: frontend sg


frontend --> public --> 0.0.0.0/0
80
CIDR

1. Bastion
2. VPN

Open source modules with AWS contribution
1. We no need to develop the module

1. We need to depend on community
2. We dont have freedom to do changes if required

Custom Modules
----------------
1. We have freedom to develop whatever we want

1. We have to develop everything

["subnet-1","subnet-2"] --> list
subnet-1,subnet-2 --> StringList

StringList --> List == ["subnet-1","subnet-2"] --> subnet[0]

ec2 instance user data
-------------------
when instanc created, AWS will run this user instructions with root access automatically


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -34
----------------------------------------------------------------------------------------------------------------------------------------------------------------
expense-infra-dev
-------------------

stateful vs stateless

stateful --> which has state, i.e data
stateless --> which don't have state.

DB --> stateful, it keeps track of the data
backend/frontend --> stateless

DB
-------
backup --> hourly, daily, weekly backups
restore test
data replication --> 
DB-1 is connected to application
DB-2 is not connected to application, but replicate data from DB-1

HYD --> DB-1, MUM --> DB-2
storage increment
load balancing
upgrades

RDS --> load balancing, auto storage increment, backups/snapshots, etc..

ExpenseApp1

8.0.35 --> 8.0.36
8.0 --> 8.1 --> 9

rds opensource module

mysql-dev.daws81s.online --> expense.czn6yzxlcsiv.us-east-1.rds.amazonaws.com

snapshot/backup --> destroy --> final snapshot(VPC)

Load Balancing and Auto Scaling
--------------------------------

DM --> Client

work --> UI work --> UI team lead --> team members
Backend work --> Backend team lead --> team members
who is available to work, team lead will assign

team --> target group
team lead --> load balancer
listener and rules --> He is listening for UI work, Backend team lead is listening for backend work
who is available --> health check
members --> servers

if backend server is running we can hit that on 8080, if not running we can't hit


8080, 80 --> servers are listening on these port

http://3.94.106.200/ --> 2XX

LB Listener --> 80 --> nginx --> 80 --> 2 instances --> lb will check which instance is healthy --> randomly 1 server

Auto scaling
------------------
2 members --> 16 hr work
30 hr work --> our HR should recruit new members --> add them to our team

JD --> Launch template(Options to create servers) --> place them inside target group

CPU utilisation --> 75%

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -35
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. Project infra --> Basement to house --> rare changes
2. Application infra --> Rooms and walls --> Yes

Project Infra
-----------------
VPC --> VPC will not change frequently
SG --> SG may not change, only rules may change
Bastion --> No
DB --> No
Load Balancer --> No

Applications
------------------
Ec2 instances
target groups


backend-dev.daws81s.online --> LB
backend-dev.daws81s.online:8080

Load Balancer
----------------
LB --> distributing the load to target group --> team lead
TG --> A team of members --> A group of servers
Server --> Team member --> Server
Listener --> Team Lead Phone number --> Port LB listening to
Rules --> 

host path and context path
------------------

Client --> BA --> Architect --> 
backend --> Backend LB --> Backend TL
frontend  --> Frontend LB --> Frontend TL
database --> DB TL

hostpath
--------------
backend.daws81s.online --> backend LB
frontend.daws81s.online --> frontend LB

Classic LB
Application LB --> Layer7
Network LB

m.facebook.com --> mobile site
facebook.com --> web site

netbanking.hdfc.com
demat.hdfc.com

context path
-----------------
daws81s.online/backend
daws81s.online/frontend

app ALB --> app tier LB
web ALB --> web tier LB


mysql-dev.daws81s.online --> expense-dev.czn6yzxlcsiv.us-east-1.rds.amazonaws.com
app-dev.daws81s.online --> App ALB
web-dev.daws81s.online --> Web ALB
daws81s.online --> domain
web-dev.daws81s.online --> subdomain

app-dev.daws81s.online
------------------------
app-dev.daws81s.online --> it will respond --> default response
backend.app-dev.daws81s.online --> forward this request to backend TG

fasdfghasfj.app-dev.daws81s.online

-------------------------------------------------------------------------------------------------------------------------------------------------------------
Session - 36
--------------------------------------------------------------------------------------------------------------------------------------------------------------
VPN
--------------
user laptop --> VPN --> can access secure servers
and company can monitor our traffic

AMI --> Open VPN server is already installed and configured

Launch this EC2 and we need to little configuration

OpenVPN Access Server Community Image-fe8020db-*

22, 943, 443, 1194 --> VPN ports

key is mandatory for this

ssh -i ~/.ssh/openvpn openvpnas@public-IP

https://35.170.248.89:943/admin
openvpn, Admin@1234

VPN SG, VPN SG Rules
create key pair for VPN access
VPN instance with Open VPN
openvpnas is the user name
configure with default options
https://35.170.248.89:943/admin
openvpn, Admin@1234

download openvpn connect
https://35.170.248.89:943
openvpn Admin@1234

Backend
--------------------------
1. create ec2 instance
2. configure with backend 

if there is a new version
-----------------
I can connect all the instances using ansible and run the playbook

stop the server
remove old code
download new code
restart the server

if there is traffic increase
-------------------
1. create ec2 instance
2. configure with backend 


create ec2 instance
configure using ansible
stop the instance
take AMI --> Launch template
launch it using autoscaling

when traffic increase
use AMI to add the servers

target group
ALB rules

------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -37
------------------------------------------------------------------------------------------------------------------------------------------------------------------

create ec2 instance
configure it using ansible
stop the server
take AMI --> with new version
delete the instance

create launch template
	ami, network, sg, etc.
create target group
create ASG using launch template and place them in TG
create rule in load balancer

create ansible server and provide backend ec2 instance
ansible can connect to it...

provisioners
	local and remote

I need to use remote provisioner, connection block

null resource and trigger

null resource --> It will not do anything, means it wont create any resource. But useful for provisioners

terraform --> Shell --> Ansible

if you have existing folder how can you make it as git repo
------------------------------------------------------------
we need to intialise git
git init

git branch -M main

aws ec2 terminate-instances --instance-ids instance-id1

for i in $(ls -dr */) ; do echo ${i%/}; cd  ${i%/} ; terraform destroy -auto-approve ; cd .. ; done

for i in $(ls -d */) ; do echo ${i%/}; cd  ${i%/} ; terraform apply -auto-approve ; cd .. ; done

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -38
----------------------------------------------------------------------------------------------------------------------------------------------------------------

Create EC2
Configure EC2 Using ANsible and provisioner
	remote provisioner
	variables in terraform --> Shell script --> ansible-pull
stopped EC2
take AMI
delete the instance

LB, Listener, Default rule

target group
launch template

:8080/health

for i in 10-vpc 20-sg 30-bastion 40-rds 50-app-alb 50-vpn; do cd $i ; terraform apply -auto-approve ; cd ..; done

2 instances --> 60 80

backend.app-dev.daws81s.online --> forward this backend target group

expense-dev.daws81s.online --> expense website

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -39
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

EC2
configure
stop
AMI
delete instance
target group
launch template
auto scaling group
autoscaling group policy

ALB Rule

R53 --> ALB --> Listener --> Rule --> Target group --> Health Check --> Instance
0 1 2 3 4

backend.app-dev.daws81s.online

app-dev.daws81s.online

catalogue.app-dev.daws81s.online
user.app-dev.daws81s.online
shipping.app-dev.daws81s.online

zeal vora --> AWS security specialist

Rolling update
-----------------
4 instances --> 2 instances

1. stop all the backend services in all instances and update the application using ansible
2. create one new instance using new version, once this is up, delete one old instance	
	create second instance and delete one more old instance
	create third instance and delete one more old  instance
	create fourth instance and delete one more old instance
	
https/SSL/TLS --> certificates

We need domain

hdfcbank.com --> https://hdfcbank.com:443

they will check authorization of your domain

certificate provider

expense-dev.daws81s.online
expense-qa.daws81s.online

*.daws81s.online
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -40
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

Create EC2
Configure it
	You should have playbooks ready
	remote provisioner
	terraform variables --> Shell --> ansible
	ansible-pull
stop ec2
take ami
delete instance

create target group
create launch template

autoscaling --> launch template target group
autoscaling policy
ALB rules

R53 --> ALB --> Listener(80,443) --> Rule(host based routing) --> target group --> Instance

ACM --> we should have domain

request for the certificate
create records in domain
validation

expense-dev.daws81s.online

504 --> Gateway timeout

ALB --> Server

domains
-----------
daws81s.online

backend.app-dev.daws81s.online --> APP ALB

expense-dev.daws81s.online

mysql-dev.daws81s.online

CDN --> Cloudfront
-------------------

user --> ISP Caching servers --> Torrents

Origin --> Where the original content exist
cache --> static content (css, js, images)

https://expense-dev.daws81s.online/static/media/3TierArch.0486e7150e53d305d1c2.png

--------------------------------------------------------------------------------------------------------------------------------------------------------------
Session -41
--------------------------------------------------------------------------------------------------------------------------------------------------------------

CDN
------------
Cloudfront is a content delivery network service of amazon. AWS have edge locations edge locations to cache the content across the globe. we can make use of this service to reduce latency to our customers...

Origin --> where is your source. It can S3, ALB, Api gateway, etc.
Cache behaviour --> How and what you want to cache
invalidations -> When there is a update, you can create invalidations so that edge location pull the content newly.

cache order
-------------
/images/* --> expense.cdn.daws81s.online/images/* --> it will cached
/static/* --> expense.cdn.daws81s.online/static/* --> it will cached
default --> dynamic content --> expense.cdn.daws81s.online --> no cache

cdn 			origin
http--> https 

http://expense-cdn.daws81s.online --> https://expense-cdn.daws81s.online 
