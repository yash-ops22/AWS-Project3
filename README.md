# Task-3

Statement: We have to create a web portal for our
company with all the security as much as possible.
So, we use Wordpress software with dedicated
database server.
Database should not be accessible from the outside
world for security purposes.
We only need to public the WordPress to clients.
So here are the steps for proper understanding!

Steps:
1) Write a Infrastructure as code using terraform, which automatically create a VPC.

2) In that VPC we have to create 2 subnets:
    a)  public  subnet [ Accessible for Public World! ] 
    b)  private subnet [ Restricted for Public World! ]

3) Create a public facing internet gateway for connect our VPC/Network to the internet world and attach this gateway to our VPC.

4) Create  a routing table for Internet gateway so that instance can connect to outside world, update and associate it with public subnet.

5) Launch an ec2 instance which has Wordpress setup already having the security group allowing  port 80 so that our client can connect to our wordpress site.
Also attach the key to instance for further login into it.

6) Launch an ec2 instance which has MYSQL setup already with security group allowing  port 3306 in private subnet so that our wordpress vm can connect with the same.
Also attach the key with the same.

Note: Wordpress instance has to be part of public subnet so that our client can connect our site. 
mysql instance has to be part of private  subnet so that outside world can't connect to it.
Don't forgot to add auto ip assign and auto dns name assignment option to be enabled.
# VPC
Amazon Virtual Private Cloud (Amazon VPC) enables you
to launch AWS resources into a virtual network that
you've defined. This virtual network closely resembles
a traditional network that you'd operate in your own
data center, with the benefits of using the scalable
infrastructure of AWS.

# Let's Go

Before creating any Resources, we have to configure 
our AWS Profile:
Here we have already configure it 

    AWS Access Key ID [****************LELI]:
    AWS Secret Access Key [****************Av0P]:
    Default region name [ap-south-1]:
    Default output format [json]:
    
Then in our terraform code specifying the 
provider for creating the resources into 
the specifide provider...

    provider "aws" {
    region     = "ap-south-1"
    profile    = "Yashu"
  }
   
# Step 1:
  Firstly, we have to create a Virtual Private Network or
  VPC so that we can launch our resurces into it....
  
  Terraform code for our VPC....
  
    resource "aws_vpc" "cloud3-vpc" {
    cidr_block       = "192.168.0.0/16"
    instance_tenancy = "default"

    tags = {
      Name = "cloud3-vpc"
    }
  }

# Step 2:
Here in this step we are creating two subnets 
one of them is public and the other is private.
Private Subnet will store our private resources
and data which is not accessible to outside world.
Public subnet will contain the resources which
will we accessible to outside world for their use.

Public Subnet...

    resource "aws_subnet" "public-subnet2" {
    vpc_id     = aws_vpc.cloud3-vpc.id
    cidr_block = "192.168.0.0/24"
    map_public_ip_on_launch  = true
    availability_zone = "ap-south-1a"

    tags = {
      Name = "public-subnet2"
    }
  }
    
Private Subnet....

     resource "aws_subnet" "private-subnet1" {
     vpc_id     = aws_vpc.cloud3-vpc.id
     cidr_block = "192.168.1.0/24"
     availability_zone = "ap-south-1a"
     
     tags = {
       Name = "private-subnet1"
     }
   }



     















