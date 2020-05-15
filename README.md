# Trigger AWS Lambda from S3

In this lab, we will create a Lambda function that will get triggered when an object is placed into an S3 bucket. Specifically, we will trigger a job within Amazon Transcribe to transcribe speech from an audio file placed into S3.

This Demo is based on LA course: [Github](https://github.com/linuxacademy/content-aws-mls-c01/tree/master/Trigger-an-AWS-Lambda-Function-from-an-S3-Event)

## Create IAM Role for Lambda
Login to AWS console and on the upon right corner select us-east-1 (Virginia) as your region.

Open IAM Service and Select [Roles](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles).

```json
Click Create role.
Select Lambda as the trusted entity.
Click Next: Permissions.
On the permissions policies page, search for (in the Filter policies box) 
and select each of the following managed policies:
- AmazonS3ReadOnlyAccess
- AmazonTranscribeFullAccess
- CloudWatchLogsFullAccess
Click Next: Tags.
Add the following tag:
  Key: creator
  Value: Enter your name
Click Next: Review.
Enter a Role name of "my-lambda-role".
Click Create role.
```

## Create a Lambda Function


Navigate to [Lambda](https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions)
```json
Click Create a function.
With Author from scratch selected, and then use the following settings:
    Function name: lab-lambda-transcribe
    Runtime: Python 3.6
Expand Choose or create an execution role, and use the following settings:
    Execution role: Use an existing role
    Existing role: my-lambda-role
Click Create function.
In the Function code section, delete the existing code and paste from the lambda_function.py file (On this repository). 
```


## Create a Trigger for the Lambda Function
Navigate to [S3](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#).
```json
Click to open the input-... bucket.
Click the Properties tab.
Click the Events card.
Click Add notification, and set the following properties:
- Name: transcribe
- Events: All object create events
- Send to: Lambda Function
- Lambda: lab-lambda-transcribe
Click Save.
```

## Test the Architecture
Download the test [Files](https://linuxacademy.com/cp/guides/download/refsheets/guides/refsheets/Trigger-an-AWS-Lambda-Function-from-an-S3-Event-LAB-FILES_1583241901.zip)
```json
Head back to the bucket.
Click Upload.
Select and upload the sample audio file from the lab files. Unzip the zip file to see the files, including the audio file.
Navigate to Lambda.
Click our function name.
Click Monitoring, where we should see CloudWatch metric populate.
Click View logs in CloudWatch.
Click the listed log stream. We should then see the beginning and end of when our Lambda function ran.

Navigate to Amazon Transcribe.
Click Transcription jobs in the left-hand menu.
The Transcribe service outputs to a service managed S3 bucket and now does not specify the Output URL, but you can just choose the Download full transcript button to download the JSON file.
```