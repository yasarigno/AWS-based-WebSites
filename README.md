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

# A Website on an EC2 Virtual Machine - a good idea as well as for building other things using a VM.

The website will be built on an EC2 instance, therefore, the server will always be running. Unlike a static website you will be paying for it. We start with Cloud9 and register a new PEM file to have a SSH connection between Cloud9 and a new EC2 instance.

1. Create a PEM file.

Go to the EC2 console ``Key Pairs`` and name a new key. Once created it will automatically download the key on your computer.

2. Upload this key on the Cloud9 environment. 

File > Upload a file... Upload the PEM file. Be sure that it is located under the main folder.


3. Create a security group.

Go to the EC2 console ``Security Groups``  In section "Inbound rules" set ``Port Range = 20`` and ``Source = 0.0.0.0/0`` (meaning it can be connected from anywhere)

Do the same for Port Range = 80.

4. Request spot instance and launch.

Go to the EC2 console ``Spot Requests`` and follow "request spot instances"

Under the section "Key pair name" select the key that you have uploaded to Cloud9 environment. Select the security group ``ec2websitegroup`` (this is the one created)  under "Additional launch parameters" 

5. On the EC2 console ``Instances`` a new instance will appear (wait and refresh if not!) Then it is better to name it as well.

Click on the new instance and in the following page ``connect``.

On SSH client tab, follow the instructions. (we have already created PEM key etc.)

