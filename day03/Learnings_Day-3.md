# Day 3 - TerraWeek

## Creating AWS EC2 instances using Terraform (IAC)

**Prerequisites**:
- AWS CLI ( Install AWS CLI on your system and configure it using your IAM user credentials)
- TerraForm ( Install TerraForm for your respective OS)

## **Main.tf** file:
```
{
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
  
  required_version = ">=1.2.0"
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "my_ec2_instance" {
  count         = 1
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"
  
  tags = {
    Name = "Terraform-created"
  }
}

output "ec2_public_ips" {
  value = element(aws_instance.my_ec2_instance.*.public_ip, 0)
}
```
### Understanding each block:
1)
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
  
  required_version = ">=1.2.0"
}
```
- This block specifies the Terraform configuration version and required provider versions. In this case, it sets the required provider for AWS to version 4.16 or higher.

2)
```
provider "aws" {
  region = "ap-south-1"
}
```
- This block configures the AWS provider, specifying the desired AWS region as ap-south-1 (Asia Pacific - Mumbai).

3)
```
resource "aws_instance" "my_ec2_instance" {
  count         = 1
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"
  
  tags = {
    Name = "Terraform-created"
  }
}
```
- This block defines an AWS EC2 instance resource. It creates an EC2 instance with the specified AMI (Amazon Machine Image), instance type, and tags. The instance will be created once, as count is set to 1.

4)
 ```
 output "ec2_public_ips" {
  value = element(aws_instance.my_ec2_instance.*.public_ip, 0)
}
```
- This block defines an output variable called ec2_public_ips. It retrieves the public IP address of the EC2 instance created earlier using the element function. The aws_instance.my_ec2_instance.*.public_ip expression references the public IP addresses of all instances created. The element function is used to select the first (and only) element from the list.



## Creating our infrastructure

Step 1 : Before we start creating our resources and executing file we will need to initialise our local repo to fetch required TerraForm modules for our main.tf file . We can do this using the command `terraform init`.

![Screenshot 2023-06-07 at 7 13 09 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/3f773abb-aa67-4a7a-abd7-005a1d9156a0)

Step 2 : After initialising we need to validate our `main.tf` file and we can do that using the command .....you guessed it! `terraform validate`. If there aren't any errors in out main file then we can proceed to next step

Step 3 : We will use the command `terraform plan` to see the what would be the steps taken by terraform once we execute our file and after going through the steps we can proceed to next step

Step 4: This is our final step of creation of our infrastructure and we will achieve it using the command `terrafrom apply` . ( we would get a prompt seeking our confirmation wheter or not we want to create our resource so type `yes` in that)

- output of Step 4:
![Screenshot 2023-06-07 at 7 19 46 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/1d2150dd-dc74-4a00-9e66-558960c652d6)

- Final Outcome :
![Screenshot 2023-06-07 at 7 22 11 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/2fcded5d-c24a-4f47-95b2-2cf206edada2)


## Looking into `terraform.tfstate` file :
- This file is located in out root directy where `main.tf` is stored and it stored the state of our infrastructure after our main file is applied 
- for example mine looked like this:
![Screenshot 2023-06-07 at 7 26 35 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/8487be56-f6c4-4216-b264-18451b466fee)

- This state changes according to the changes made by terraform and preserves the history of our actions so that the process of creating infrastructure becomes seamless.

## modifying `main.tf` :
- let's say you don't want a instance anymore so in that case you can simply set the value of `count` to `0` and the created instance would be destroyed and accordingly the state of terraform would be changed too.
- Now after running the command  `terraform plan` you can confirm that it's going to destroy/terminate a instance:

![Screenshot 2023-06-07 at 7 32 48 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/c3bdd004-32a5-4d73-b8c1-30b1b550a94e)

- Using the command `terraform apply` we can apply our modification and our instance would be terminated.

## Adding provisioner 
- A provisioner allows us to run scripts or perform actions on a resource after it is created or destroyed
- We can add it to our script by adding a provisioner block inside our terraform block 
```
resource "aws_instance" "my_ec2_instance" {
  count         = 1
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-created"
  }

  provisioner "local-exec" {
    command = "echo 'Configuration completed'"
  }
}
```
- Now to `destroy` the resources or remove them we can use the command `terraform destroy`:

![Screenshot 2023-06-07 at 7 43 24 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/6a481a24-88fb-4cab-bf18-9f96bbab1e10)



