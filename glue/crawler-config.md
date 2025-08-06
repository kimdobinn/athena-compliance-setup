# Glue Crawler Configuration

**Name**: `dynamodb-to-athena-crawler`  
**IAM Role**: `GlueCrawlerAthenaRole`  
**Source**: DynamoDB Table (`respiree-data-processing-xxxx`)  
**Output Database**: `defaultgluedatabase`  
**Schedule**: On-demand  
**Recrawl Policy**: Crawl all files (default)
