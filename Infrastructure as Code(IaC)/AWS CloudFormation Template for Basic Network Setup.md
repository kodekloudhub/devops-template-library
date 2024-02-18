## AWS CloudFormation Template: Basic Network Setup

This CloudFormation template is designed to quickly establish a foundational network infrastructure within AWS, consisting of a Virtual Private Cloud (VPC), a subnet, an Internet Gateway, and a Security Group.

### CloudFormation YAML Template with Placeholders

Copy the YAML content below into a file named `basic-network-setup.yaml`, replacing `<Placeholder-Values>` with actual values that match your project's requirements.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Basic Network Setup - Replace with your project description

Resources:
  # Define a Virtual Private Cloud (VPC) for your project
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: "<Your-VPC-CIDR-Block>" # Example: 10.0.0.0/16 - Define the IP range for the VPC
      EnableDnsSupport: true              # Enables DNS support within the VPC
      EnableDnsHostnames: true            # Allows instances in the VPC to have DNS hostnames
      Tags:
        - Key: Name
          Value: "<Your-VPC-Name>"       # Example: MyProjectVPC - Name your VPC for easier identification

  # Create a subnet within your VPC
  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC                   # Reference to your VPC defined above
      CidrBlock: "<Your-Subnet-CIDR-Block>" # Example: 10.0.1.0/24 - Define the IP range for the subnet
      AvailabilityZone: "<Your-Availability-Zone>" # Specify the AZ, e.g., us-west-2a
      MapPublicIpOnLaunch: true           # Assign a public IP to instances launched in this subnet
      Tags:
        - Key: Name
          Value: "<Your-Subnet-Name>"     # Example: MyProjectSubnet - Name your subnet

  # Internet Gateway to connect your VPC to the internet
  MyInternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: "<Your-Internet-Gateway-Name>" # Example: MyProjectInternetGateway - Name your IGW

  # Attach the Internet Gateway to your VPC
  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref MyVPC                   # Reference to your VPC
      InternetGatewayId: !Ref MyInternetGateway # Reference to your Internet Gateway

  # Security Group to define access rules for your instances
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: "Allow HTTP and SSH access"
      VpcId: !Ref MyVPC                   # Associate this security group with your VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: "0.0.0.0/0"             # SSH access - Consider restricting to known IPs
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: "0.0.0.0/0"             # HTTP access - Adjust as necessary for your application
```

### How to Deploy

To deploy your AWS infrastructure:

1. **Customize Your Template**: Replace all `<Placeholder-Values>` in `basic-network-setup.yaml` with actual data relevant to your project. This includes specifying your desired CIDR blocks, names for your resources, and the availability zone for your subnet.

2. **Launch CloudFormation Stack**:
    - Go to the AWS Management Console.
    - Access the CloudFormation service.
    - Select "Create stack" > "With new resources (standard)".
    - Upload the `basic-network-setup.yaml` file.
    - Proceed through the stack creation wizard, providing any required details, and then create the stack.

3. **Verify Your Infrastructure**: After the CloudFormation stack creation completes, verify in the AWS Management Console that all resources were successfully created and configured as expected.
