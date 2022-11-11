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

{
    "Version": "2012-10-17",
    
    "Statement": [
    
        {
        
            "Sid": "VisualEditor0",
            
            "Effect": "Allow",
            
            "Action": [
            
                "polly:*",
                
                "s3:PutAccountPublicAccessBlock",
                
                "s3:GetAccountPublicAccessBlock",
                
                "s3:ListAllMyBuckets",
                
                "s3:HeadBucket"
            ],
            
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            
            "Effect": "Allow",
            
            "Action": "s3:*",
            
            "Resource": [
                "arn:aws:s3:::my-talking-app",
                
                "arn:aws:s3:::my-talking-app/*"
                
            ]
        }
    ]
}

Copy the ARN of the IAM role and add it under the provider section of the serverless.yml file.

provider:
   name: aws
   
   runtime: nodejs8.10
   
   region: us-east-1 
   
   role: arn:aws:iam::885121665536:role/talking-app-role
   
#  “Speak” Lambda Function
Speak Lambda function does three main tasks.

Call AWS Polly synthesizeSpeech API and get the audio stream (mp3 format) for text that user entered

Save the above audio stream in the S3 bucket

Get a signed URL for the saved mp3 file in the S3 and send it back to the frontend application
First of all, let’s install the required npm modules inside the backend folder. 

npm install aws-sdk 

npm install uuid
AWS Polly synthesizeSpeech API requires the text input and the voice id to convert the text into speech. 
Here, we use the voice of “Joanna” to speak the text that is passed from the frontend.

Now, deploy the backend API and the Lambda function

sls deploy

# Frontend Angular App
In order to test our backend, we need a frontend that makes speak request with user inputted text. 
So let’s create an angular application.

ng g s API

Add the following code to the api.service.ts file.
It will create speak function that call the lambda function with the selected voice and 
the inserted text by the user.

Add the following code to the api.service.ts file. 
It will create speak function that call the lambda function with the selected voice and 
the inserted text by the user.

Since we are using, ngModel in the app.
component.html we need to import the FormsModule in the app.module.ts file. 
Goto app.module.ts file and replace the content with,

# Running the Application
Now that our backend and the frontend are ready, let’s play with our app.

Go to the client directory and run the angular app locally,

ng serve



