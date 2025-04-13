# Infrastructure State Backend

This repository contains an AWS CloudFormation template to set up infrastructure for managing Terraform/OpenTofu remote state. It includes resources for S3 bucket storage, DynamoDB table for state locking, and IAM roles for state management and infrastructure operations.

## Resources

### S3 Bucket
- **Name**: `infrastructure-state-${AWS::AccountId}`
- **Features**:
  - Versioning enabled
  - Default encryption using AES256

### DynamoDB Table
- **Name**: `infrastructure-state-lock`
- **Purpose**: To manage state locking for distributed plan and apply operations

### IAM Roles
1. **State Management Role**:
   - Permissions to modify objects in the S3 bucket and update the DynamoDB lock table.
   - Trust policy allows the role `arn:aws:iam::${AWS::AccountId}:role/ops-admin` to assume it.

2. **Infrastructure Operator Role**:
   - Attached with AWS `AdministratorAccess` policy.
   - Trust policy allows the role `arn:aws:iam::${AWS::AccountId}:role/ops-admin` to assume it.

## Usage

1. Validate the CloudFormation template:
   ```bash
   aws cloudformation validate-template --template-body file://backend.yml
   ```

2. Deploy the stack:
   ```bash
   aws cloudformation create-stack --stack-name infrastructure-state-backend --template-body file://backend.yml --capabilities CAPABILITY_NAMED_IAM
   ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
