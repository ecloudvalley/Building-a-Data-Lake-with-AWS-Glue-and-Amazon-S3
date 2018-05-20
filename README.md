Building a Data Lake with AWS Glue and Amazon S3
================================================================


## Scenario
The following procedures help you set up a data lake that could store and analyze data that addresses the challenges of dealing with massive volumes of heterogeneous data. A data lake allows organizations to store all their data—structured and unstructured—in one centralized repository. Because data can be stored as-is, there is no need to convert it to a predefined schema. This tutorial walks you define a database, configure a [crawler](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html) to explore data in an Amazon S3 bucket, create a table, transform the CSV file into Parquet, create a table for the Parquet data, and query the data with Amazon Athena.


## Architecture Diagram
AWS Glue is an essential component of an Amazon S3 data lake, providing the data catalog and transformation services for modern data analytics.

![s3-glue-data-lake.gif](/images/s3-glue-data-lake.gif)


## Prerequisites

>Make sure your are in US East (N. Virginia), which short name is us-east-1.


## Lab tutorial
#### Create IAM role

1.1. 	On the **service** menu, click **IAM**.

1.2. 	In the navigation pane, choose **Roles**.

1.3. 	Click **Create role**.

1.4. 	For role type, choose **AWS Service**, find and choose **Glue**, and choose **Next: Permissions**.

1.5. 	On the **Attach permissions policy** page, search and choose **AWSS3FullAccess**, **AWSGlueServiceRole**, and choose **Next: Review**.

1.6. 	On the **Review** page, enter the following detail:

* **Role name: AWSGlueServiceRoleDefault**

1.7. 	Click **Create role**.

1.8. 	Choose **Roles** page, select the role **AWSGlueServiceDefault** you just created.

1.9. 	On the **Permissions** tab, choose the link **add inline policy** to create an inline policy.

1.10. 	On the JSON tab, paste in the following policy:

	{
  		"Version": "2012-10-17",
  		"Statement": [
    	{
      		"Effect": "Allow",
      		"Action": [
        		"logs:CreateLogGroup",
        		"logs:CreateLogStream",
        		"logs:PutLogEvents",
        		"logs:DescribeLogStreams"
    	],
      		"Resource": [
        		"arn:aws:logs:*:*:*"
    		]
  		}
 		]
	}

1.11. 	Click **Review policy**.

1.12. 	On the Review policy, enter policy name: **AWSCloudWatchLogs**.

1.13. 	Click **Create policy**.

1.14. 	Now confirm you have policies as below figure.


![IAM role policies.png](/images/IAM role policies.png)


#### Add Crawler

1.15. 	On the **Services** menu, click **AWS Glue**.

1.16. 	In the console, choose **Add database**. In the **Database name**, type **nycitytaxi**, and choose **Create**.

1.17. 	Choose **Crawlers** in the navigation pane, choose **Add crawler**. Add type Crawler name **nytaxicrawler**, and choose **Next**.

1.18. 	On the **Add a data store** page, choose **S3** as data store.

1.19. 	Select **Specified path in my account**.

1.20. 	Enter data store path **s3://aws-bigdata-blog/artifacts/glue-data-lake/data/**, and choose **Next**.

1.21. 	On **Add another data store** page, choose **No****, and choose **Next**.

1.22. 	Select Choose an existing IAM role, and choose the role  **AWSGlueServiceRoleDefault** you just created in the drop-down list, and choose **Next**.

1.23. 	For **Frequency**, choose **Run on demand**, and choose **Next**.

1.24. 	For **Database**, choose **nycitytaxi**, and choose **Next**.

1.25. 	Review the steps, and choose **Finish**.

1.26. 	The crawler is ready to run. Choose **Run it now**.

1.27. 	When the crawler has finished, one table has been added. Choose **Tables** in the left navigation pane, and then choose **data** to confirmed.

![AWS Glue table has been added.png](/images/AWS Glue table has been added.png)

![table information.png](/images/table information.png)
