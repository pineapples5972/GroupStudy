---
tags:
  - Terraform
  - DevOps
---

References: 
1. Course video - https://www.youtube.com/watch?v=YcJ9IeukJL8&t=3014s
2. Labs - [https://bit.ly/YT_Terraform](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2psbU52SXhoSkxNZEg0TDJxd2YybzhtNlVld3xBQ3Jtc0trV3hzMWlRQ01ETXFzN1JhS2tlcHRBTmRETnU0UlVCU3AySUxqeVhVNjJjbmp4YUNWN0Z2VWYybG5NVDh3M2taY3V1N0ZaX1cyNUhpcF9HREYwc0VfYWNFS3RxcnZId3BNci16TUxVSGU1Y1E0MHV4SQ&q=https%3A%2F%2Fbit.ly%2FYT_Terraform&v=YcJ9IeukJL8)

### What is Terraform?
 - A solution for Infrastructure as Code (IaC) 
 - Helps to deploy your own cloud infrastructure.
 - You can add many services like ec2, s3, docker kubernetes with one click
 - Uses HCL 

![[./metadata/attachments/Images/Pasted image 20231202161526.png|Pasted image 20231202161526.png]] In Traditional way you want to build an Infrastructure you have to go through manual method deploying each and every component one by one and configuring them which would be very tedius task and difficult to achieve between limited time space.
While Infrastructure as Code Helps you to be focus on mapping out architecture of Infrastructure in Code format so with click or command you can deploy reproduceable and scalable infrastructure within seconds of time.

![[./metadata/attachments/Images/Pasted image 20231215144608.png|Pasted image 20231215144608.png]]

### Types of IAC Tools
![[./metadata/attachments/Images/Pasted image 20231215145513.png|Pasted image 20231215145513.png]]

Different Providers wanted to solve the problem Scripting IAC with their own tools or proprietary scripting language.
In this race they made their own platforms and methods. 
Collectively these set of Tools called as IAC tools, each tool addressing the specific IAC need.

IAC can be classified into three major common types:
  - Configuration Management
  - Server Templating
  - Provisioning Tools

We'll see each of these in a bit detail:
#### Configuration Management:
These includes Ansible, puppet, saltstack
  - Designed to install and manage software
  - Maintain Standard Structure
  - Version Control
  - Idempotent
#### Server Templating
 These includes Docker, Packer, Vagrant
- Preinstalled Software and Dependancies
- Virtual Machines or Docker Images
- Immutable Infrastructure

#### Provisioning Tools
These includes Terraform and Cloud Formation
- Deploy Immutable Infrastructure resources
- Server, Databases, Network Components, etc
- Multiple Providers

Terraform leverages the use of APIs across various platform provider to deploy the infrastructure.

![[./metadata/attachments/Images/Pasted image 20240116142023.png|Pasted image 20240116142023.png]]
### HCL - Hashicorp Configuration Language
Using programming scripts or shell scripts to deploy architecture is not always everyones cup of tea and not neccessary everyone are well equiped with skills require to do so.
Therefore HCL Hashicorp Configuration Language Provides Easy to use and intuitive way to create your infrastucture with one click.
  

![[./metadata/attachments/Images/Pasted image 20240116140803.png|Pasted image 20240116140803.png]]

```.tf
resource "aws_instance" "webserver" {
	ami           = "ami-0edab43b6fa892279"
	instance_type = "t2.micro"
}

resource "aws_s3_bucket" "finance" {
	bucket = "finance-21092020"
	tags   = {
		Description = "Finance and Payroll"
		}
}
resource "aws_iam_user" "admin-user" {
	name = "lucy"
	tags = {
		Description = "Team Leader"
	}
}
```

This sample code is use to provision a new ec2-instance on aws cloud.
This code is declarative and can be maintain in a version control system allowing to be distributed to other teams.
#### Keypoints
The code we defined is a desired state that we want our infrastructure to be in.
but before deploying this code the state of our infrastructure is empty and there is nothing so what does the terraform do to transform the empty state into desired state.

Terraform works in three phases:
1. Init
2. Plan
3. Apply

##### Init phase:
During the init phase terraform initializes the project and identifies the providers used for the target enviroment.

##### Plant Phase:
During the plan phase terraform draws plan to get to the target state.

##### Apply Phase
During the apply phase terraform makes the neccessary changes to the target to bring it to the desired state.

##### Resource
Every object that Terraform manages is called as resource. 
A resource can be a file, vm, or could be services like ec2 instance and s3 bucket, dynamodb table, Iam users, Iam groups, roles policies, etc.

![[./metadata/attachments/Images/Pasted image 20240116155220.png|Pasted image 20240116155220.png]]
##### State
Terraform records the state of infrastructure as it is seen the real world and based on this it can determine what actions to take to updating resources for a particular platform.

It make sure that infrastructure is always in the defined state at all times.
The state is blueprint of the infrastructure defined by terraform.

