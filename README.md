# aws-cf-cloudera-cluster
AWS CloudFormation Template to Deploy a Cloudera Manager Cluster

The CloudFormation template will deploy a preconfigured Cloudera Manager cluster.

Assumptions:

1. The user has root access to his/her AWS console

2. The user has provisioned two preconfigured AM!s-- one with the Cloudera Amanager binaries and prerequisites, and one for the CDH nodes.

 Explanation:

 This CloudFormation template creates the infrastructure for a CDH cluster by creating the following resources:

- Internet Gateway
- VPC
- Route Table
- VPC Gateway Attachment
- Subnet
- Security Group
- Route
- Subnet Route Table Association
- IAM Role
- IAM Role Profile
- AutoScaling Launch Configuration
- EC2 instances
- AutoScaling Group

The following edits need to be made:

Mappings:EC2RegionMap

 This section of the template needs to be changed to identify the user's AMIs (names and AMI-IDs) and the region in which they  reside.  

- Unless, "us-east-1" is your region, change it to match your region.
- Change "cdhimg" : "ami-3e521529" to match the name and ami-id of your CDH AMI
- Change "cmimg" : "ami-3a52152d" to match the name and ami-id of your CM AMI

CMInstance:Properties:ImageId

 In this section, the ImageId needs to be changed to match the key that references the Cloudera Manager AMI (cmimg) that was set in the EC2RegionMap:

CMinstance:Properties:UserData

 After the stack is created, this section executes any user defined scripts.  In this snippet, the script /opt/init/initcm.sh is executed.  Initcm.sh can do anything imaginable-- download a repository file, grab updates, make programmatic changes to Cloudera Manager, etc.

The /bin/true statement was added to prevent the stack creation from failing in the event the user forgets to add the initcm.sh script.

LaunchConfig:Properties:ImageID

 The LaunchConfig section is responsible for deploying the machines that will be the CDH nodes.  The ImageID value (cdhimg) needs to be updated with the name of the AMI you've designated as the image from which you will create your CDH nodes.  This is the key you designated in the Mappings section.

LaunchConfig:Properties:UserData

 After the stack is created, this section executes any user defined scripts.  In this snippet, the script /opt/init/initcdh.sh is executed.  Initcdh.sh can do anything imaginable-- download a repository file, grab updates, install packages, etc.

 The /bin/true statement was added to prevent the stack creation from failing in the event the user forgets to add the initcm.sh script.  








