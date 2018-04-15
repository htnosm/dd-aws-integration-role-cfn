# dd-aws-integration-role-cfn
Datadog AWS Integration IAM Role and Policy cloudformation template

## Reference

- [Datadog AWS Integration](https://docs.datadoghq.com/integrations/amazon_web_services/#installation)

## Usage

Download [template file](IAM/DatadogAWSIntegrationRole.template.yaml)

### 1. Create Stack

- Upload template.yaml to CloudFormation

<img src="images/README/11_create-stack.png" width="512">

- Choose permissions to grant when uploading

<img src="images/README/12_create-stack_choose.png" width="512">

- Get IAM RoleName

<img src="images/README/13_get_rolename.png" width="512">

#### CLI

```
# create-stack
aws cloudformation create-stack \
  --stack-name DatadogAWSIntegrationRoleStack \
  --template-body file://IAM/DatadogAWSIntegrationRole.template.yaml \
  --capabilities CAPABILITY_IAM

# Wait for create-stack to complete
aws cloudformation wait stack-create-complete \
  --stack-name DatadogAWSIntegrationRoleStack

# Get IAM Role Name
aws cloudformation describe-stack-resource \
  --stack-name DatadogAWSIntegrationRoleStack \
  --logical-resource-id DatadogAWSIntegrationRole \
  --query 'StackResourceDetail.PhysicalResourceId'
```

### 2. Generate ExternalID on Datadog

Generate ExternalID on Datadog AWS Integration page.

<img src="images/README/21_generate_externalid.png" width="256">

### 3. Update Stack

- Update CloudFormation Stack
  - "DatadogExternalID" specifies the generated ExternalID

<img src="images/README/31_update-stack.png" width="512">

#### CLI

```
# update-stack
## "DatadogExternalID" specifies the generated ExternalID
aws cloudformation update-stack \
  --stack-name DatadogAWSIntegrationRoleStack \
  --template-body file://IAM/DatadogAWSIntegrationRole.template.yaml \
  --capabilities CAPABILITY_IAM \
  --parameters "ParameterKey=DatadogExternalID,ParameterValue=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

# Wait for update-stack to complete
aws cloudformation wait stack-update-complete \
  --stack-name DatadogAWSIntegrationRoleStack
```
