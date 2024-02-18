## Python Script to Automate EC2 Instance Creation

This Python script showcases how to automate the creation of an Amazon EC2 instance using the `boto3` library. It's designed for simplicity and efficiency, allowing users to programmatically launch instances with specified parameters such as the AMI ID, instance type, and key pair name.

### Python Script: `create_ec2_instance.py`

Below is the script that provides a function to create an EC2 instance. It includes basic error handling and prints out the instance ID upon successful creation.

```python
import boto3

def create_ec2_instance(image_id, instance_type, keypair_name):
    ec2 = boto3.resource('ec2')
    try:
        instance = ec2.create_instances(
            ImageId=image_id,
            MinCount=1,
            MaxCount=1,
            InstanceType=instance_type,
            KeyName=keypair_name
        )
        print(f"EC2 Instance {instance[0].id} created")
        return instance[0].id
    except Exception as e:
        print(f"Error creating EC2 instance: {e}")
        return None

# Example Usage
image_id = 'ami-12345'  # Replace with actual AMI ID for your region
instance_type = 't2.micro'
keypair_name = 'your-keypair-name'  # Replace with your existing keypair name

instance_id = create_ec2_instance(image_id, instance_type, keypair_name)
if instance_id:
    print(f"Instance created successfully: {instance_id}")
else:
    print("Instance creation failed.")
```

### How to Use

1. **Install Boto3**: Ensure the `boto3` library is installed in your Python environment.
   ```bash
   pip install boto3
   ```
2. **Configure AWS Credentials**: Make sure your AWS credentials are configured properly, typically through the AWS CLI or by setting environment variables.
3. **Customize the Script**: Modify the `image_id`, `instance_type`, and `keypair_name` in the "Example Usage" section to match your requirements.
4. **Run the Script**: Execute the script to create an EC2 instance. The instance ID will be printed upon successful creation.

### Script Explanation

- **Boto3 EC2 Resource**: Uses `boto3.resource('ec2')` to interface with the EC2 service.
- **Error Handling**: Includes a try-except block to catch and print errors that may occur during instance creation.
- **Function Parameters**:
  - `image_id`: The AMI ID of the instance to launch.
  - `instance_type`: The type of instance (e.g., `t2.micro`).
  - `keypair_name`: The name of the key pair to associate with this instance for SSH access.

### Best Practices

- **Security**: Always review AWS best practices for security, especially regarding key pair management and instance access.
- **Resource Cleanup**: Remember to stop or terminate instances you no longer need to avoid unnecessary charges.
- **AMI Selection**: Ensure the AMI ID (`image_id`) is valid for the region you are launching the instance in and meets your application requirements.
