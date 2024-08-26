# AWS JSON Tag Files
The JSON files in this repository provide templates for user-defined tags. The files are formatted to support tag assignment via the Resource Groups Tagging API, CloudFormation templates deployed through the AWS CLI, or CloudFormation templates run in AWS CodePipeline.

## AWS Tagging Overview
Tags are unique key-value pairs assigned to AWS resources. There are two types of tags: user-defined and AWS generated. AWS creates and assigns AWS generated tags, which begin with the prefix "aws:". AWS generated tags cannot be modified.

User-defined tags are assigned by users and help organizations monitor and control resource state, usage, cost, and access. AWS best practices encourage implementing user-defined tags to categorize resources based on department, environment, application, and other metadata categories. Each resource can have a maximum of 50 user-defined tags.    

The AWS whitepaper [Tagging Best Practices](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html) provides guidelines for developing organizational resource tagging strategies.

## Description Of The JSON Tag Files
JSON tag files are collections of user-defined tags. Because JSON tag files can be reviewed and version-controlled, their use reduces the potential for tagging errors inherent to manually adding tags via the AWS Console or Tag Editor. Using JSON tag files is also more efficient than defining tags within CloudFormation templates, since templates require that tag sets be coded for every resource.

There are three JSON tag files included in this repository:

+ [api-tags.json](./api-tags.json): used with the Resource Groups Tagging API
+ [cfn-tags.json](./cfn-tags.json): used with CloudFormation templates deployed through the AWS CLI
+ [codepipeline-template-config.json](./codepipeline-template-config.json): used with CloudFormation templates run in AWS CodePipeline

AWS recommends that organizations apply both mandatory and discretionary tags to all resources. (See [Choosing tags for your environment](https://docs.aws.amazon.com/whitepapers/latest/establishing-your-cloud-foundation-on-aws/choosing-tags.html).) Each of the JSON tag files contains baseline mandatory tags, along with example values. The tag key-value pairs should be modified as necessary to meet organizational tagging requirements, including adding any applicable discretionary tags.

The tags contained in the JSON tag files are as follows:

| Tag | Description | Key | Example Value |
|:-----------------|:------------|:--------|:--------|
| Owner | Owner and main user of the resource. | Owner | Customer Loyalty Team |
| Business Unit | Business Unit to which the resource belongs. | BusinessUnit | Marketing |
| SDLC Stage | Indicates production vs. non-production status of the resource. | SDLCStage | Development |
| Cost Center | Budget or account that will be used to pay for the resource. | CostCenter | 12345 |
| Financial Owner | Specifies who is responsible for the costs associated with the resource. | FinancialOwner | Marketing |
| Compliance Framework | Identifies resources that are associated with a compliance framework. | ComplianceFramework | HIPPA |

## Deployment Instructions

JSON tag files can be assigned via the Resource Groups Tagging API, CloudFormation templates deployed through the AWS CLI, or CloudFormation templates run in AWS CodePipeline.

### Resource Groups Tagging API

Use the Resource Groups Tagging API to add tags to existing resources.

1. Download and save the [api-tags.json](./api-tags.json) file to either a local folder or an S3 bucket.
2. Open the file in an editor and add the Amazon Resource Names (ARNs) for the resources to which the tags will be added. Please refer to [Amazon Resource Names (ARNs)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) for instructions on finding resource ARNs.
3. Customize the tag keys and values as appropriate. 
4. Using the AWS CLI, run the code listed below. The CLI user account must have read permissions to the source bucket if the file is stored in S3.

```bash
aws resourcegroupstaggingapi tag-resources --cli-input-json file:///path/api-tags.json
```

### CloudFormation Templates Deployed Through The AWS CLI

Follow the instructions below when adding tags to resources that will be created through a CloudFormation template deployed through the AWS CLI.

1. Download and save the [cfn-tags.json](./cfn-tags.json) file to either a local folder or an S3 bucket.
2. Open the file in an editor and customize the tag key-value pairs as needed.
3. Using the AWS CLI, run the command listed below. The CLI user account must have read permissions to the source bucket if the files are stored in S3.

**NOTE:** The CLI command below creates a stack using the CloudFormation template [cfn-multi-az-vpc.yaml](https://github.com/smscully/cfn-multi-az-vpc/blob/main/cfn-multi-az-vpc.yaml) from the [cfn-multi-az-vpc](https://github.com/smscully/cfn-multi-az-vpc/) repository. Update the file path and template name as necessary.

```bash
aws cloudformation create-stack --stack-name multi-az-vpc-test\
--template-body file:///path/cfn-multi-az-vpc.yaml \
--parameters file:///path/parameters.json \
--tags file:///path/cfn-tags.json
```

### CloudFormation Templates Run In AWS CodePipeline

AWS CodePipeline can create a continuous delivery workflow for CloudFormation templates, helping automate the creation of stacks and resources. Tags are added to each of the resources using a CodePipeline template configuration file. The [codepipeline-template-config.json](./codepipeline-template-config.json) file contains the AWS mandatory tags, along with sample values.

For a walkthrough of using Cloudformation with CodePipeline, see [Continuous delivery with CodePipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/continuous-delivery-codepipeline.html). A description of the template configuration file used to define the tags is provided in [AWS CloudFormation artifacts](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/continuous-delivery-codepipeline-cfn-artifacts.html).

## License
Licensed under the [GNU General Public License v3.0](./LICENSE).
