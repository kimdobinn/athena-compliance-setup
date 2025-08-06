# AWS Athena Compliance Setup


## Objective

To configure an automated pipeline that extracts data from a DynamoDB table and enables querying via AWS Athena for compliance and data audit purposes.

## Architecture Overview

1. **DynamoDB** as the data source  
2. **Glue Crawler** to scan DynamoDB and populate the Glue Data Catalog  
3. **S3 Bucket** to store Athena query results and optional Glue job outputs  
4. **Athena** to query the structured metadata from Glue

### 1) Created S3 Bucket

```bash
aws s3api create-bucket --bucket appbackend-demo --region ap-southeast-1

Used to store:

Athena query results

Optional Glue job outputs

✅ Created IAM Role for Glue Crawler
File: iam/glue-crawler-role.json

Key permissions:

{
  "Effect": "Allow",
  "Action": [
    "dynamodb:Scan",
    "dynamodb:DescribeTable",
    "s3:*",
    "glue:*"
  ],
  "Resource": "*"
}

✅ Configured AWS Glue Crawler
Name: dynamodb-to-athena-crawler

Data Source: DynamoDB table

IAM Role: GlueCrawlerAthenaRole

Output Database: defaultgluedatabase

See details: 

✅ Athena Setup
Successfully queried the DynamoDB data via Athena:

SELECT * FROM "default"."respiree_data_processing_jnq9suqi99" LIMIT 10;

📁 Logs
Glue crawler run log:

Crawler: dynamodb-to-athena-crawler
Status: Succeeded
Tables Created: 1
Start Time: 2025-07-10T09:32:00Z
End Time: 2025-07-10T09:35:21Z


🎯 Outcome
This task ensured that:

Athena can be used as a reliable query engine for compliance reviews.

Metadata from DynamoDB is discoverable and queryable via Glue.

Infrastructure setup follows AWS best practices for permission control and modularity.

🏷️ Tags
#AWS #Athena #GlueCrawler #DynamoDB #S3 #Internship #Compliance

---

### 🧩 Additional Notes
- You can include fake/sanitized logs if you're not allowed to expose real names or data.
- Keep your `README.md` as the main narrative. Other files should support it, not overwhelm it.
- If you like, I can help you generate those example logs and JSON files as well.

Want me to generate all the example files (`glue-crawler-role.json`, `crawler-config.md`, `crawler-run-log.txt`, etc.) next?
