# AWS CUR Resource Analyzer  

Amazon QuickSight resources to be used along with  [CUDOS Cloud Intelligence Dashboards](https://github.com/aws-samples/aws-cudos-framework-deployment) to analyze AWS Cost and Usage Reports.

## Dependencies  

This stack <ins>must be</ins> installed after CUDOS has been successfully deployed. It depends on previously configured CUR data catalog capabilities and should be installed in the same account as the Cloud Intelligence Dashboards.

## Installation

Users should follow the directions in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) to create a stack from the CloudFormation console using the [cur_resource_analyzer_cloudformation.json](https://github.com/jay-giametta/AWS-CUR-ResourceAnalyzer/blob/main/cloudformation/cur_resource_analyzer_cloudformation.json) file in this repository.

## Parameters  

- **CurDbName** - The name of the CUR database. This information can be found in Athena.
- **CurTableName** - The name of the CUR table. This information can be found in Athena.
- **CurAppNameCol** - The name of the CUR column containing application name tag info. This information can be found in Athena. Leave as ```product_servicecode``` if app-level tagging is unavailable.

## Other Information
**Account Groups**  
Groups in the analysis refer to account groups that are split by grouping linked accounts that have similar names. The groupings depend on the presence of basic string delimeters and use the following QuickSight transform: ```toLower(split(replace({account_name},':','-'),'-',1))```.

**QuickSight Permissions**  
QuickSight users will need to be granted access to thew newly created assets. Directions for this process can be found in the [Amazon Quicksight User Guide - Managing assets](https://docs.aws.amazon.com/quicksight/latest/user/manage-qs-assets.html) section.

## Disclaimer

This code was created for educational purposed and should be analyzed, modified and tested in your own environment prior to production use.
