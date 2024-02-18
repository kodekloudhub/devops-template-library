## AWS CloudFormation Template: Basic Network Setup

This CloudFormation template is designed to quickly establish a foundational network infrastructure within AWS, consisting of a Virtual Private Cloud (VPC), a subnet, an Internet Gateway, and a Security Group.

### CloudFormation YAML Template with Placeholders

Copy the YAML content below into a file named `basic-network-setup.yaml`, replacing `<Placeholder-Values>` with actual values that match your project's requirements.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Basic Network Setup - Customize Description

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: "<Your-VPC-CIDR-Block>" # Example: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: "<Your-VPC-Name>" # Example: MyProjectVPC

  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: "<Your-Subnet-CIDR-Block>" # Example: 10.0.1.0/24
      AvailabilityZone: "<Your-Availability-Zone>" # Example: us-west-2a
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: "<Your-Subnet-Name>" # Example: MyProjectSubnet

  MyInternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: "<Your-Internet-Gateway-Name>" # Example: MyProjectInternetGateway

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref MyVPC
      InternetGatewayId: !Ref MyInternetGateway

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: "Allow HTTP and SSH access"
      VpcId: !Ref MyVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: "0.0.0.0/0" # Customize if needed to restrict SSH access
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: "0.0.0.0/0" # Customize if needed to restrict HTTP access
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
