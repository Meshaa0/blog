+++
author = "TechMesha"
title = "Create AWS VPC with Subnets using Terraform - Infra As Code"
date = "2024-03-20"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "VPC",
    "terraform",
    "AWS",
]
categories = [
    "themes",
    "syntax",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

<a href="https://ibb.co/ZHz2sBS"><img src="https://i.ibb.co/cbhY56w/AWS-VPC-with-Subnets.png" alt="AWS-VPC-with-Subnets" border="0"></a><br />
In this article, I'll walk you through the simple steps of creating an AWS Virtual Private Cloud (VPC) with both public and private subnets using Terraform, while ensuring we follow the best practices.

## AWS VPC Component

Below is a list of components we are going to create:

- One VPC
- One private subnets 
- One public subnets 
- One internet gateway for public internet traffic
- One route table
- One route table association 

## Let’s Start!
#### 1. Create your working folder

I have chosen *AWS VPC - Terraform*

#### 2. Create your provider

<a href="https://ibb.co/cNr3psv"><img src="https://i.ibb.co/30Cd8Ps/Screenshot-from-2024-03-20-22-17-45.png" alt="Screenshot-from-2024-03-20-22-17-45" border="0"></a>

Let's start by creating a file called "provider.tf". This file will specify that all our infrastructure will be hosted on AWS. If you ever want to switch to a different cloud provider like Google Cloud Platform (GCP) or Microsoft Azure, you'll need to update this file accordingly.


We are ready to init!

After initialization, your project folder should contain two files and one folder.


#### 3. Create your vpc

{{< highlight html >}}

resource "aws_vpc" "myvpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    name = "MyTerraformVPC"
  }
}
{{< /highlight >}}

#### 4. Create your public subnet

{{< highlight html >}}

resource "aws_subnet" "PublicSubnet" {
  vpc_id     = aws_vpc.myvpc.id
  cidr_block = "10.0.1.0/24"
  tags = {
    Name = "subnet1"
  }
}
{{< /highlight >}}

#### 5. Create your private subnet

{{< highlight html >}}

resource "aws_subnet" "PrivateSubnet" {
  vpc_id     = aws_vpc.myvpc.id
  cidr_block = "10.0.2.0/24"
  tags = {
    Name = "subnet2"
  }
}
{{< /highlight >}}

#### 6. Create internet gateway

{{< highlight html >}}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.myvpc.id
}
{{< /highlight >}}

#### 7. Create route table for public subnet

{{< highlight html >}}

resource "aws_route_table" "PublicRT" {
  vpc_id = aws_vpc.myvpc.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
}
{{< /highlight >}}

#### 8. Create route table association for public subnet 

{{< highlight html >}}

resource "aws_route_table_association" "PublicRTassociation" {
  subnet_id      = aws_subnet.PublicSubnet.id
  route_table_id = aws_route_table.PublicRT.id
}
{{< /highlight >}}

<a href="https://ibb.co/qrKk9GJ"><img src="https://i.ibb.co/2kxgP2Z/Screenshot-from-2024-03-20-22-17-29.png" alt="Screenshot-from-2024-03-20-22-17-29" border="0"></a>


Once you've completed the previous step, head over to your terminal and run the command "terraform plan" to ensure that everything is set up correctly and there are no issues.

After verifying the plan, you can proceed by executing "terraform apply" to implement the changes and create your AWS VPC with the specified configurations.

<a href="https://ibb.co/JjMfPrm"><img src="https://i.ibb.co/GQbKjCn/Screenshot-from-2024-03-20-22-18-22.png" alt="Screenshot-from-2024-03-20-22-18-22" border="0"></a>

## Now go to your AWS Console

we can see here that our VPC is working successfuly 

<a href="https://ibb.co/YbxCSh6"><img src="https://i.ibb.co/hCS6NVk/Screenshot-from-2024-03-20-22-14-19.png" alt="Screenshot-from-2024-03-20-22-14-19" border="0"></a>

Here, our 2 subnets are also working successfuly 

<a href="https://ibb.co/XVB1HxB"><img src="https://i.ibb.co/3kX2KmX/Screenshot-from-2024-03-20-22-14-54.png" alt="Screenshot-from-2024-03-20-22-14-54" border="0"></a>

Route table 

<a href="https://ibb.co/ctYnkx4"><img src="https://i.ibb.co/3WRZ4kg/Screenshot-from-2024-03-20-22-15-54.png" alt="Screenshot-from-2024-03-20-22-15-54" border="0"></a>

And lastly your  internet gateway 

<a href="https://ibb.co/fnKymfx"><img src="https://i.ibb.co/mXY2Qkc/Screenshot-from-2024-03-20-22-16-22.png" alt="Screenshot-from-2024-03-20-22-16-22" border="0"></a>

## Final Thoughts

In conclusion, setting up an AWS VPC with both public and private subnets using Terraform can be a straightforward process when following best practices and utilizing variables for flexibility. By carefully planning and executing each step, you can create a well-structured and efficient infrastructure environment tailored to your specific needs.. Happy Terraforming!
