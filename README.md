# Overview
- infraDemo.py is a python script that helps to decommission AWS resources in your account. The script is built to resolve AWS resource dependencies, it can delete all resources in a single run.
- main.tf is a terraform script that helps to deploy resources quickly so you can focus on testing your infraDemo.py script.
# Requirements to run python script:
- Boto3
- alive_progress

infraDemo.py script use config session from Boto3, the profile_name='htduong' is AWS profile configured in your laptop (or remote server where you run this infraDemo.py script). Make sure to set it to match your working environment.

For detailed packages, please look at file requirements.txt

# Terraform to build test infra
You need to create terraform.tfvars with similar content:

- access_key = ""
- secret_key = ""
- region     = "us-east-1"

Then run terraform against main.tf file.
The main.tf script will create VPCs, Instances, TGW, TGW VPC Attachment, TGW Connect, Security Groups, Internet Gateway.
Remember to set the tag to AWS resources. The tag here is extremely important as the infraDecom.py script will look at the tag to build inventory of AWS resources first before decommissioning.

The tag using in main.tf script:

tags = {
    Name = "vpc1"
    AciOwnerTag = "huyen"
  }

The custom filter in infraDecom.py script:

custom_filter = [
    {
        'Name': 'tag:AciOwnerTag',
        'Values': ['?*']
    }
]

Tag key in main.tf must matches tag Name in custom_filter. In this example, both are using same value: AciOwnerTag

# Run the script to decommission AWS resources:

python3 infraDecom.py

# References

https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html
https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html
