# Data Pipeline S3 (CSV) to RDS Postgres copy 

## Overview

This sample shows how to build a data pipeline which copies data csv records from s3 to an RDS Postgres database. 

## Prerequisites

You must have the AWS CLI and default IAM roles setup in order to run the sample. Please see the [readme](https://github.com/awslabs/data-pipeline-samples) for the base repository for instructions how to do this.

You must also provide the S3Path of a S3 bucket with write permissions. See [here](http://docs.aws.amazon.com/AmazonS3/latest/UG/CreatingaBucket.html) for instructions on how to create an S3 bucket.

Apart from this, please note the following:

The EC2 resource launched by datapipeline should have ingress access to the RDS database. In this case, since it is a postgres database, you will have to allow ingress access to the RDS instance from the ec2 resource at port 5432 (default port for postgres). 

  As an example, let's say the rds instance has a security group attached sg-xxxxx. You can create or use another security group sg-yyyy for the ec2-resource. You will have to then provide 5432 port access to sg-xxxx for sg-yyyyy. 

  Additionally, you will also need to have a subnet id where you would like to launch the ec2 resource. 

It's important to know that the S3DataNode object will have a DataFormat. In this case this will be a CSV type Data Format. You will need to add columns based on your table to in the definition. In my tests, I just used one column, but you can add more columns depending on your data. 

Please review this and apply the appropriate values below based on your environment. 

```
"values": {
    "myRdsInstanceId": "<your-rds-instance-id>",
    "myUsername": "<your-db-user-name>",
    "myFilePath": "<s3://your-input-location>",
    "mySecurityGroupIds": "<your-security-gorup-id-for-ec2>",
    "mySubnetId": "<your-subnet-id-for-ec2>",
    "myDatabaseName": "<your-database-name>",
    "mySchemaName": "<your-schema-name>",
    "myTablename": "<your-table-name>",
    "myPipelineLogUri": "<s3://your-log-location>",
    "*myPassword": "<your-db-password>"
  }
```

You will also need to add more columns and modify the below part of the definition: 

```
{
"name": "DataFormat",
"column": "id",
"id": "DataFormatId_4wPQK",
"type": "CSV"
},
```

Once this updated, you can either use the CLI or the "Import a Definition" option in the datapipeline dashboard to upload the datapipeline and activate it.

## CLI Steps 
```
> aws datapipeline create-pipeline  --name <datapipeline-name>  --unique-id 1

 {
     "pipelineId": "df-xxxxxxxxxxxxx"
 }

> aws datapipeline put-pipeline-definition \
 --pipeline-id df-xxxxxxxxxxxxx \
 --pipeline-definition file://S3ToPostgresRDS.json 


> aws datapipeline activate-pipeline \ 
 --pipeline-id df-xxxxxxxxxxxxx
```

## Disclaimer

The samples in this repository are meant to help users get started with Data Pipeline. They may not be sufficient for production environments. Users should carefully inspect code samples before running them.

Use at your own risk.

Copyright 2011-2013 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Licensed under the Amazon Software License (the "License"). You may not use this file except in compliance with the License. A copy of the License is located at

http://aws.amazon.com/asl/
