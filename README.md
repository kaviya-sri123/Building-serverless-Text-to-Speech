# Building-serverless-Text-to-Speech
Building Serverless Text - to - Speech application using Amazon Polly and Amazon Amplify

# Creating a Serverless Project/Service
Install serverless framework by with npm and create a new nodejs project/service called backend.

npm install serverless -g 
serverless create --template aws-nodejs --path backend

Now replace to serverless.yml file with following code, that creates a lambda function called “speak”.

# Creating an S3 Bucket
We need an S3 bucket to store all the voice clips that are returned by AWS Polly. Use AWS console to create
the bucket with a unique name. In my case, S3 bucket name is “my-talking-app”.

# Create an IAM Role
The serverless framework creates two Lambda functions that interact with AWS Polly and AWS S3 services.
In order to communicate with these services, our Lambda function must be assigned an IAM role that has permission 
to talk to S3 and Polly. So, create an IAM role with a preferred name i.e. “talking-app-role” with the
following IAM policy.

Copy the ARN of the IAM role and add it under the provider section of the serverless.yml file.

provider:
   name: aws
   runtime: nodejs8.10
   region: us-east-1 
   role: arn:aws:iam::885121665536:role/talking-app-role






