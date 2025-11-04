<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Endpoints

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-endpoints)

**Author:** Paul Ndukwe  
**Email:** paulnd95@gmail.com

---

## VPC Endpoints

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_09bcaa8a)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a virtual network in AWS that lets you launch resources in an isolated, secure environment. It gives you full control over network settings, enhances security, and enables private connections between your AWS resources.

### How I used Amazon VPC in this project

I used Amazon VPC to setup a VPC endpoint, specifically the S3 gateway. This provides our VPC with direct private access to another AWS service (S3).

### One thing I didn't expect in this project was...

I did not expect my access to my S3 bucket through the console to be blocked once I had set up my bucket policy to deny all access except through the endpoint.

### This project took me...

This project took 80mins to complete.

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I’m building the core infrastructure for the project by creating a new VPC, launching an EC2 instance to run and test from, and setting up an S3 bucket for storing data. This foundation lets me securely connect and test AWS services lat

### Step 2 - Connect to EC2 instance

Connect to your EC2 instance via SSH to test access to S3 over the public internet. This shows the default behavior before setting up a VPC endpoint, helping you compare connectivity and security before and after the endpoint is added.

### Step 3 - Set up access keys

In this step, I'm going to give my EC2 instance access to my AWS environment by creating access keys and configuring them on the instance. This allows it to authenticate and interact with AWS services like S3 programmatically.


### Step 4 - Interact with S3 bucket

In this step, I'm going to head back to my EC2 instance and use the access keys I set up to access my S3 bucket. This will show that my instance can successfully connect to AWS services using those credentials.


---

## Architecture set up

I started my project by launching a VPC with 1 public subnet, with access to the internet through an internet gateway. I then launched an EC2 instance within my public subnet after creating a new security group for it. 

I also set up an Amazon S3 bucket and uploaded two files in it.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_4334d777)

---

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I configured the Access Key ID, secret access key, default region and default output format.

Access keys are a pair of security credentials (an access key ID and a secret access key) used to authenticate programmatic requests to AWS services via the AWS CLI, SDKs, or APIs. They are associated with an IAM user or role.


Secret access keys are the confidential part of an AWS access key pair, used alongside the access key ID to securely sign and authenticate API requests to AWS services. They should be kept private and never shared or exposed.


### Best practice

Although I'm using access keys in this project, a best practice alternative is to use **IAM roles**. IAM roles provide temporary credentials and eliminate the need to store long-term access keys on your instance, improving security and manageability.

---

## Connecting to my S3 bucket

The command I ran was 'aws s3 ls'. This command is used to list all the s3 buckets within my account, i.e., all buckets I have access to.

The terminal responded with the single S3 bucket in my account. This indicated that the access keys I set up works!

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_4334d778)

---

## Connecting to my S3 bucket

I also tested the command 'aws s3 ls s3://nextwork-vpc-endpoints-pauldukey-95' which returned the files within my S3 bucket.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_4334d779)

---

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command 'sudo touch /tmp/nextwork.txt'. This command creates an empty text file named nextwork.txt in the temporary /tmp/ directory on my EC2 instance.

The second command I ran was 'aws s3 cp /tmp/nextwork.txt s3://nextwork-vpc-endpoints-pauldukey-95'. This command will copy the empty file nextwork.txt from my EC2 instance to my specified S3 bucket.


The third command I ran was 'aws s3 ls s3://nextwork-vpc-endpoints-pauldukey-95', which validated that the file nextwork.txt was successfully uploaded by listing the contents of my S3 bucket.


![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_3e1e79a2)

---

## In the second part of my project...

### Step 5 - Set up a Gateway

I’m going to set up a VPC endpoint to create a private connection between my VPC and Amazon S3. This means my EC2 instance can access the S3 bucket securely without sending data over the public internet, reducing exposure to potential threats.

### Step 6 - Bucket policies

In this step, I’m creating a bucket policy that restricts access to only my VPC endpoint. This blocks all other traffic, ensuring my EC2 instance accesses S3 securely through the endpoint and confirming no data travels over the public internet.

### Step 7 - Update route tables

In this step, I’m going to test if my EC2 instance can still access the S3 bucket now that I’ve restricted access to only my VPC endpoint. This will confirm whether the endpoint is working correctly or if there’s an issue I need to troubleshoot.


### Step 8 - Validate endpoint conection

In this step, I’m testing if my EC2 instance can still access the S3 bucket through the VPC endpoint to confirm the setup works. Then, I’ll tighten security by restricting my VPC’s access to only trusted AWS resources for a safer environment.


---

## Setting up a Gateway

I set up an S3 Gateway, which is a type of VPC endpoint that acts as a gateway to route traffic directly between my VPC and Amazon S3. It enables private, secure access to S3 without using the public internet.


### What are endpoints?

An endpoint is a private connection point that enables secure communication between your VPC and AWS services without using the public internet. It allows resources in your VPC to access services like S3 directly and privately.


![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_09bcaa8a)

---

## Bucket policies

A bucket policy is a JSON-based access control policy attached to an S3 bucket that defines who can access the bucket and what actions they can perform, helping to securely manage permissions and control access to the bucket’s contents.


My bucket policy will restrict access to my S3 bucket so only requests coming through my VPC endpoint are allowed, blocking all other traffic and ensuring secure, private communication between my VPC and the bucket.


![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_7316a13d)

---

## Bucket policies

Right after saving my bucket policy, my S3 bucket showed 'denied access' warnings because the policy blocks all traffic except from my VPC endpoint. Any requests not using the endpoint, including public internet, are denied to secure the bucket.

I also had to update my route table because my route table by default didn't provide a route for traffic in my public subnet to the VPC endpoint.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_4ec7821f)

---

## Route table updates

To update my route table, I visited the endpoints page on my VPC console and modified the route table to associate my VPC's public subnet 

After updating my public subnet's route table, my terminal could return a list of S3 bucket contents, confirming that my EC2 instance can access the bucket through the correct network routes.


![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_d116818e)

---

## Endpoint policies

An endpoint policy is a type of policy designed to specify the range of resources and actions permitted by an endpoint.

I updated my endpoint's policy by changing the effect from 'Allow' to 'Deny'. I could see the effect of this right away, because my EC2 instance was again denied access to S3 when I tried to run an AWS S3 list command.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-networks-endpoints_3e1e79a3)

---

---
