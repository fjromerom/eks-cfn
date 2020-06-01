# EKS-CFN

This project deploys an EKS cluster version 1.16 with the following features.

- Cluster endpoint access is private and only accessible through the VPC
- EKS worker nodes are deployed by Managed Node Groups
- Cluster Autoscaler is enabled by default
- EFS storage class can be enabled optionally
- An EC2 instance is also deployed and preconfigured with kubectl and helm to manage the EKS cluster
- There is no SSH access. The EC2 instances are managed through SSM

### Required Input Parameters

The eks-master.yaml template requires the following input parameters.

#### Network configuration

- **VPCID**. The ID of an existing VPC.
- **PrivateSubnet1ID**. The ID of the private subnet in Availability Zone 1.
- **PrivateSubnet2ID**. The ID of the private subnet in Availability Zone 2.
- **PrivateSubnet3ID**. The ID of the private subnet in Availability Zone 3.

#### EKS configuration

- **ManagedNodeGroupAMIType**. The AMI type used for the EKS Managed Node Group. It can be standard or with GPU support.
Default value: AL2_x86_64
- **NodeInstanceType**. The EC2 instance type for the EKS worker nodes. Bear in mind that if you select GPU AMI type the instance type must be GPU supported, i.e. g4dn.xlarge.
Default value: m5.xlarge
- **NumberOfNodes**. The number of EKS worker nodes. This value can be increased after the creation of the stack.
Default value: 3
- **MaxNumberOfNodes**. The maximum number of EKS worker nodes. This value can be increased after the creation of the stack.
Default value: 6

#### Optional Kubernetes add-ins

- **EfsStorageClass**. If enabled it creates an EFS filesystem and deploys the efs-provisioner to mount the EFS filesystem as persistent volumes.
Default vale: Disabled

#### Assets location

- **S3BucketName**. The S3 bucket name for the assets.
- **S3BucketRegion**. The AWS Region where the bucket is hosted.
- **S3KeyPrefix**. The S3 key prefix for the assets.
