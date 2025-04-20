# learn-aws-cloudformation-tableau-server
how to deploy tableau server in aws for windows

To deploy the `tsm-universal-template-win2022-v1.yaml` CloudFormation template from your repository, follow these steps:

### Prerequisites:
1. **AWS CLI Installed**:
   Ensure the AWS CLI is installed and configured with the proper credentials and region.
   - Install instructions: [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
   - Configure using: `aws configure`

2. **IAM Permissions**:
   Ensure your user or role has sufficient permissions to create and manage AWS resources like EC2, S3, etc.

3. **Template Validation**:
   Validate the template to ensure there are no errors:
   ```bash
   aws cloudformation validate-template --template-body file://tsm-universal-template-win2022-v1.yaml
   ```

---

### Deployment Steps:

#### 1. Upload the Template to an S3 Bucket (Optional)
If the template is large or you wish to manage it in S3:
   ```bash
   aws s3 cp tsm-universal-template-win2022-v1.yaml s3://<your-s3-bucket-name>/
   ```
   Note the S3 URL of the template (e.g., `s3://<your-s3-bucket-name>/tsm-universal-template-win2022-v1.yaml`).

#### 2. Deploy the Template
Run the following command to create a CloudFormation stack:

If the template is local:
```bash
aws cloudformation create-stack --stack-name tableau-server-stack \
    --template-body file://tsm-universal-template-win2022-v1.yaml \
    --parameters ParameterKey=InstanceName,ParameterValue=tableau-server \
                 ParameterKey=InstanceType,ParameterValue=r5.4xlarge \
                 ParameterKey=KeyPairName,ParameterValue=<your-key-pair-name> \
                 ParameterKey=VpcId,ParameterValue=<your-vpc-id> \
                 ParameterKey=SubnetId,ParameterValue=<your-subnet-id> \
                 ParameterKey=VolumeSizeGB,ParameterValue=100 \
    --capabilities CAPABILITY_NAMED_IAM
```

If the template is in S3:
```bash
aws cloudformation create-stack --stack-name tableau-server-stack \
    --template-url https://<your-s3-bucket-name>.s3.amazonaws.com/tsm-universal-template-win2022-v1.yaml \
    --parameters ParameterKey=InstanceName,ParameterValue=tableau-server \
                 ParameterKey=InstanceType,ParameterValue=r5.4xlarge \
                 ParameterKey=KeyPairName,ParameterValue=<your-key-pair-name> \
                 ParameterKey=VpcId,ParameterValue=<your-vpc-id> \
                 ParameterKey=SubnetId,ParameterValue=<your-subnet-id> \
                 ParameterKey=VolumeSizeGB,ParameterValue=100 \
    --capabilities CAPABILITY_NAMED_IAM
```

Replace `<your-key-pair-name>`, `<your-vpc-id>`, and `<your-subnet-id>` with your actual AWS resources.

#### 3. Monitor the Stack Creation
Monitor the stack creation using the AWS CLI:
```bash
aws cloudformation describe-stacks --stack-name tableau-server-stack
```
Or in the AWS Management Console under **CloudFormation**.

#### 4. Access the Deployed Resources
Once the stack is created successfully:
- Use the outputs (such as EC2 Instance ID, Public IP) to access the instance.
- Ensure you have added appropriate security group rules for accessing the EC2 instance (e.g., SSH, RDP).

---
### Resource 
### https://www.tableau.com/support/releases/server/2024.2.9#esdalt
Windows - https://downloads.tableau.com/esdalt/2024.2.9/TableauServer-64bit-2024-2-9.exe

Linux - https://downloads.tableau.com/esdalt/2024.2.9/tableau-server-2024-2-9.x86_64.rpm
