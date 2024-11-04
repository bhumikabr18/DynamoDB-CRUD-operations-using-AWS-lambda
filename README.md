# DynamoDB-CRUD-operations-using-AWS-lambda
This project demonstrates how to perform basic CRUD (Create, Read, Update, Delete) operations on an AWS DynamoDB table using AWS Lambda and the Python boto3 library. 
All operations are tested directly within AWS Lambda.

# Demonstration Video
https://drive.google.com/file/d/1XyZYcKrENa_h8pWeK7CTMGyDs0jZIZHP/view?usp=drivesdk

# Overview
This project uses AWS Lambda to perform the following operations on a DynamoDB table:

1. Create a new student record.
2. Read a student's information.
3. Update an existing student record.
4. Delete a student record.
<br>
There is no API Gateway involved; all testing can be done within the AWS Lambda console or via AWS CLI.

# Prerequisites
Before using this project, you will need:

* AWS Account: To create and manage AWS services (Lambda, DynamoDB).
* IAM Role: Ensure your Lambda function has permissions to access DynamoDB (dynamodb:PutItem, dynamodb:GetItem, dynamodb:UpdateItem, dynamodb:DeleteItem).
* Python 3.x: Lambda supports Python 3.8 and later versions.

# DynamoDB Table Structure
The table used in this project is called studentsCreation, and it has the following structure:

* Table Name: studentsCreation
* Partition Key: studentId (String)

# How to Run
* Create the DynamoDB Table: Follow the table structure described above in the DynamoDB Table Structure section.
* Deploy the Lambda Function: Upload the code to AWS Lambda and configure the function with the appropriate IAM role and permissions.
* Test the Function: Use the AWS Lambda console or CLI to test your Lambda function by providing the necessary event payloads.

# Error Handling
Common errors and troubleshooting:

* AccessDeniedException: Ensure that your Lambda function has the correct IAM role with permissions for DynamoDB.
* ResourceNotFoundException: Make sure the DynamoDB table exists and the table name in your Lambda code is correct.
* ValidationException: Ensure the data passed to DynamoDB conforms to the table's schema (e.g., correct data types for studentId, age, etc.).






  
