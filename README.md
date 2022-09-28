# AWS based WebSites
I build multiple websites: 
  - static (on S3),
  - serverless (with AWS Lambda),
  - virtualized (on an EC2 virtual machine),
  - PaaS (AWS Beanstalk)

## A Static S3 Website on AWS

The idea is to put all .html files on S3 and give access to public. A file on S3 is private unless you open it to everyone in the world. Once this is done the set of files we created on S3 will form a complete website.

1. The files should be created in AWS CLoud9. For now we create a simple .html file.

````
touch index.html
````

2. Create a bucket on S3. Give it a name (Mine is "static-website-bucket-fy") and under the section "Block Public Access settings for this bucket" enable all public access. Acknowledge that this is the change.

3. We want this bucket to host a website. Click on the bucket name and then under the section "properties" you will see the section "static website hosting". Edit. Define the index document as ``index.html`` and the error document as ``error.html`` 

4. Time to add a bucket policy to correctly serve out traffic. Go to "Permission" and there edit the Bucket policy by setting the policy for websites [here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html). In this policy do not forget to replace the bucket's name by yours.

5. Now dowload the files of your website from CLoud9 and upload them to your S3 bucket.

6. To check if the website works, go to "Properties" and you find the url of the website under the section "static website hosting"

http://static-website-bucket-fy.s3-website.eu-central-1.amazonaws.com/

## A Serverless Website on AWS Lambda

Here we build a website on which whenever we click on an URL, it will call the function of AWS Lambda. 

1. We write a python function on AWS Lambda. Click on ``Create function`` and choose ``Author from scratch``. Because it will be a python function, choose the runtime as Python 3.9

```python

import json

def lambda_handler(event, context):
    content = """
    <html>
    <p> My first Lambda website </p>
    </html>
    """
    response = {
        "statusCode": 200,
        "body": content,
        "headers": {"Content-Type": "text/html",},
    }
    return response

```

2. After ``Deploy``ing the code, we add a ``trigger``. In the configuration page choose API Gateway and then HTTP API. Security: Open.

3. Saving it will open the main page and there (API Gateway) we find: 

https://6g6dmvg209.execute-api.eu-central-1.amazonaws.com/default/WebFunction



