Main participant: [Sarah Elnagar](https://app.nuclino.com/users/1f317638-efc4-4361-a031-3883f34588f5?mId=tpj76H6l)

Create S3 bucket "[route53rangers](https://us-east-1.console.aws.amazon.com/s3/buckets/route53rangers?region=us-east-1)" and upload the media we need to be shared through the webapp

**Managing S3 access**

- From S3 bucket permission ,  allow "***all*** **public access" **

  ![6a0f1ccb-73f1-4b09-84c5-9cf19fae7743.png](https://files.nuclino.com/files/c9ed9899-9893-4eab-9632-fb77169ac848/6a0f1ccb-73f1-4b09-84c5-9cf19fae7743.png)
- Create S3 policy from "Policy generator" to allow any "principle" to "GetObject"
  ```
  {
  	"Version": "2012-10-17",
  	"Id": "Policy1726179419756",
  	"Statement": [
  		{
  			"Sid": "Stmt1726179418294",
  			"Effect": "Allow",
  			"Principal": "*",
  			"Action": "s3:GetObject",
  			"Resource": "arn:aws:s3:::route53rangers/*"
  		}
  	]
  }
  ```
- Assign instance profile with Role to allow EC2 to "ListBucket" & "GetObject"
  ```
  - Effect: Allow
    Action:
      - s3:ListBucket
      - s3:GetObject
    Resource:
      - arn:aws:s3:::route53rangers
      - arn:aws:s3:::route53rangers/*
  ```
  To display media from S3; add the media path from S3 into userdata inside the index.html file of the application
  ```
  <img src="https://route53rangers.s3.amazonaws.com/beach.jpg" alt="Image 1" width="400">
  <img src="https://route53rangers.s3.amazonaws.com/coffee.jpg" alt="Image 2" width="400">
  ```
