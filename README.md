# AWS-CUR-ResourceAnalyzer  

Amazon QuickSight resources to be used along with  [CUDOS Cloud Intelligence Dashboards](https://github.com/aws-samples/aws-cudos-framework-deployment) to analyze AWS Cost and Usage Reports.

## Dependencies  

This stack <ins>must be</ins> installed after CUDOS has been successfully deployed. It depends on previously installed CUR data catalog information.

## Parameters  

- **CurDbName** - The name of the CUR database. This information can be found in Athena.
- **CurTableName** - The name of the CUR table. This information can be found in Athena.
- **CurAppNameCol** - The name of the CUR column containing application name tag infoa. This information can be found in Athena. Leave as 'product_servicecode' if app-level tagging is unavailable.

## Other Information
**Account Groups**  

Groups in the analysis refer to account groups that are created by grouping linked accounts that have similar names. The groupings depend on the presence of basic string delimeters and use the following QuickSight transform: ```toLower(split(replace({account_name},':','-'),'-',1))```.

## Disclaimer

This code was created for educational purposed and should be analyzed, modified and tested in your own environment prior to production use.
