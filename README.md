# AWS CUR Resource Analyzer  
Amazon QuickSight resources to be used along with  [CUDOS Cloud Intelligence Dashboards](https://github.com/aws-samples/aws-cudos-framework-deployment) to analyze AWS [Cost and Usage Report (CUR)](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html) Data Exports.

## Dependencies  
This stack **<ins>must be</ins>** installed after CUDOS has been successfully deployed. It depends on [automated CUR updates and ingestion](https://catalog.workshops.aws/well-architected-cost-optimization/en-US/2-expenditure-and-usage-awareness/60-automated-cur-updates-and-ingestion) from the Cloud Intelligence Dashboards and should be installed in the same account.

## Installation
Users should follow the directions in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) to create a stack from the CloudFormation console using the [cur_resource_analyzer_cloudformation.json](cloudformation/cur_resource_analyzer_cloudformation.json) file in this repository.

[<img src="https://static.us-east-1.prod.workshops.aws/public/7049bac4-6e2b-4642-a834-f86cc8c523fd/static/LaunchStack.png" width="175">](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https%3A%2F%2Faws-cur-resourceanalyzer.s3.us-east-1.amazonaws.com%2Fcur_resource_analyzer_cloudformation.json&stackName=cur-resource-analyzer&param_CurAppNameCol=product_servicecode&param_CurDbName=cur&param_CurTableName=customer_all)

## CloudFormation

### Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `CurDbName` | The name of the CUR database. This information can be found in Athena. | "cur" |
| `CurTableName` | The name of the CUR table. This information can be found in Athena. | "customer_all" |
| `CurAppNameCol` | The name of the CUR column containing application name tag info. This information can be found in Athena. Leave as```product_servicecode```if app-level [tagging](https://docs.aws.amazon.com/tag-editor/latest/userguide/best-practices-and-strats.html) is unavailable. | "product_servicecode" |

**Note**: See the following AWS Well-Architected Cost Optimization Workshop for help with [Cost and Usage Analysis - SQL](https://catalog.workshops.aws/well-architected-cost-optimization/en-US/2-expenditure-and-usage-awareness/70-cost-and-usage-analysis-sql) in Athena.

### Resources

#### 1. CurDataSource (AWS::QuickSight::DataSource)
- Creates a QuickSight data source connected to Athena

#### 2. ResourceAnalyzerDataset (AWS::QuickSight::DataSet)
Creates a dataset with three main SQL queries:
- `account_map`: Maps account IDs to account names and parent accounts
- `resource_analyzer`: Main query extracting cost and usage data
- `app_first_seen`: Tracks when applications were first seen in billing data

#### 3. ResourceViewAnalysis (AWS::QuickSight::Analysis)
Creates a QuickSight analysis with two sheets:

##### Sheet 1: "Application Info"
Shows cost distribution by application. Includes visualizations for:
- Costs by date first seen
- 30-day costs by application
- 30-day costs by group
- Cost details in a pivot table
- Application dates
- Number of apps by date first seen

##### Sheet 2: "AWS Product/Resource Info"
Shows detailed AWS product and resource information. Includes visualizations for:
- 30-day costs by operation
- 30-day costs by product family
- AWS product/resource costs in a detailed pivot table

### Features

- Interactive filtering capabilities
- Parameter controls for:
  - Application
  - Operation
  - Product family
  - Account group
- Date range filtering
- Drill-down capabilities
- Currency formatting
- Customized layouts and visual settings

## Other Information
**Account Groups**  
Groups in the analysis refer to account groups that are split by grouping linked accounts that have similar names. The groupings depend on the presence of basic string delimeters and use the following [QuickSight function](https://docs.aws.amazon.com/quicksight/latest/user/toLower-function.html):```toLower(split(replace({account_name},':','-'),'-',1))```.

**QuickSight Permissions**  
QuickSight users will need to be granted access to the newly created assets. Directions for this process can be found in the [Amazon Quicksight User Guide - Managing assets](https://docs.aws.amazon.com/quicksight/latest/user/manage-qs-assets.html) section.

## Screenshots
![screenshot](images/resource_analyzer_1.jpg)

## Disclaimer
This code was created for educational purposes and should be analyzed, modified and tested in your own environment prior to any production use.
