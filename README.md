# 🧠 AWS Athena Compliance Setup

## 📌 Objective

To configure an automated pipeline that extracts data from a DynamoDB table and enables querying via AWS Athena for compliance and data audit purposes.

---

## 🗺️ Architecture Overview

1. **DynamoDB** as the data source  
2. **AWS Glue Crawler** to scan DynamoDB and populate the Glue Data Catalog  
3. **S3 Bucket** to store Athena query results and optional Glue job outputs  
4. **Athena** to query the structured metadata from Glue

```plaintext
                  +--------------------+
                  |   DynamoDB Table   |
                  | respiree-data-...  |
                  +--------------------+
                            |
                            v
                +------------------------+
                |    Glue Crawler        |
                | dynamodb-to-athena-...|
                +------------------------+
                            |
                            v
              +----------------------------+
              | Glue Data Catalog / DB     |
              |  defaultgluedatabase       |
              +----------------------------+
                            |
                            v
                +------------------------+
                |      AWS Athena        |
                |   (Query Interface)    |
                +------------------------+
                            |
                            v
                +------------------------+
                |     S3 Bucket          |
                |  appbackend-demo       |
                +------------------------+
```

---

## 🛠️ Work Planning

### 1. ✅ S3 Bucket Setup

- Created S3 bucket for staging environment: `s3://appbackend-demo`
- Purpose:
  - Store Athena query results
  - Optional output location for future Glue jobs

```bash
aws s3api create-bucket --bucket appbackend-demo --region ap-southeast-1
```

---

### 2. ✅ IAM Role for Glue Crawler

Created IAM role: `GlueCrawlerAthenaRole` with the following:

- **Trust Policy**: Allows Glue service to assume this role
- **Inline Policy**:

```json
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:Scan",
    "dynamodb:DescribeTable",
    "dynamodb:ListTables",
    "s3:*",
    "glue:*"
  ],
  "Resource": "*"
}
```

➡️ [View full JSON here](iam/glue-crawler-role.json)

---

### 3. ✅ Glue Crawler Configuration

- **Name**: `dynamodb-to-athena-crawler`  
- **Data Source**: DynamoDB table `respiree-data-processing-ygdn0ixv0d`  
- **IAM Role**: `GlueCrawlerAthenaRole`  
- **Output Database**: `defaultgluedatabase`  
- **Optional Output**: `s3://appbackend-demo`

➡️ [View crawler config](glue/crawler-config.md)

---

### 4. ✅ Athena Setup

Successfully connected Athena to the Glue Catalog and ran a sample query:

```sql
SELECT * FROM "default"."respiree_data_processing_jnq9suqi99" LIMIT 10;
```

- Data source: `azprod-dataprocessing-dynamodb`
- Glue database: `default`
- Table: `respiree-data-processing-ygdn0ixv0d`

➡️ [View schema](glue/glue-database.md)  
➡️ [View query](sql/sample-query.sql)

---

## 📄 Logs

Glue crawler execution log:

```text
Crawler: dynamodb-to-athena-crawler
Status: Succeeded
Tables Created: 1
Start Time: 2025-07-10T09:32:00Z
End Time: 2025-07-10T09:35:21Z
```

➡️ [View full log](logs/crawler-run-log.txt)

---

## ✅ Outcome

This setup ensured that:

- Athena can reliably query structured data extracted from DynamoDB.
- Metadata is discoverable and queryable via the Glue Catalog.
- The pipeline follows AWS best practices in terms of modularity, security, and automation-readiness.

---

## 🏷️ Tools Used

- AWS Glue
- AWS Athena
- Amazon S3
- AWS IAM
- DynamoDB
- SQL (Athena dialect)
