## Terraform Template for AWS Infrastructure

This Terraform template outlines how to create a basic AWS network infrastructure, including a VPC, subnet, internet gateway, EC2 instance, and S3 bucket. The template is annotated with comments to provide insights into each configuration step.

### AWS Terraform Configuration with Comments

Below is the `main.tf` file content, featuring detailed comments:

```hcl
# Specify the provider and define your AWS region
provider "aws" {
  region = "<Your-AWS-Region-Here>" # Example: us-west-2
}

# Create a Virtual Private Cloud (VPC) to provide an isolated network
resource "aws_vpc" "example_vpc" {
  cidr_block = "10.0.0.0/16"
  enable_dns_hostnames = true

  tags = {
    Name = "example-vpc" # Customize the VPC name
  }
}

# Create a subnet within your VPC
resource "aws_subnet" "example_subnet" {
  vpc_id            = aws_vpc.example_vpc.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "<Your-Availability-Zone-Here>" # Example: us-west-2a

  tags = {
    Name = "example-subnet" # Customize the subnet name
  }
}

# Create an Internet Gateway for connecting your VPC to the internet
resource "aws_internet_gateway" "example_igw" {
  vpc_id = aws_vpc.example_vpc.id

  tags = {
    Name = "example-igw" # Customize the Internet Gateway name
  }
}

# Deploy an EC2 instance within your subnet
resource "aws_instance" "example_instance" {
  ami           = "<Your-AMI-ID-Here>" # Example: ami-0c55b159cbfafe1f0
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.example_subnet.id

  tags = {
    Name = "example-instance" # Customize the instance name
  }
}

# Create an S3 bucket for object storage
resource "aws_s3_bucket" "example_bucket" {
  bucket = "<Your-Unique-Bucket-Name-Here>" # Ensure this name is globally unique
  acl    = "private"
}
```

### How to Use

To deploy this AWS infrastructure with Terraform:

1. **Customize Your Configuration**: Replace all placeholder values (<Your-Value-Here>) with actual data relevant to your AWS setup. This includes specifying the AWS region, availability zone, AMI ID for the EC2 instance, and a unique name for your S3 bucket.

2. **Save Your Configuration**: Copy the provided Terraform configuration into a file named `main.tf` within your Terraform project directory.

3. **Initialize Terraform**:
   Run `terraform init` to initialize the Terraform workspace, which downloads the AWS provider plugin.

    ```bash
    terraform init
    ```

4. **Review the Plan**:
   Use `terraform plan` to review the actions Terraform will perform before making changes to your infrastructure.

    ```bash
    terraform plan
    ```

5. **Apply the Configuration**:
   Deploy your infrastructure by running `terraform apply` and approve the action when prompted.

    ```bash
    terraform apply
    ```

6. **Check Your Resources**:
   After the apply completes, verify the creation of the resources in your AWS account through the AWS Management Console or CLI.

By following these steps and utilizing the annotated `main.tf` file, you can easily set up a basic AWS infrastructure tailored for your applications, leveraging Terraform's infrastructure as code capabilities for efficient and reproducible deployments.
