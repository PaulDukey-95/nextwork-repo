<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Infrastructure as Code with CloudFormation

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-cloudformation-updated)

**Author:** Paul Ndukwe  
**Email:** paulnd95@gmail.com

---

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

## Introducing Today's Project!

In this project, I will demonstrate how to set up a CloudFormation template.  I'm doing this project to learn about infrastructure as code, and how I can apply this to my CI/CD and future architecture. By the end of this project, resources like CodeDeploy, CodeConnection and more will be defined together in a single template.

### Key tools and concepts

Services I used were CloudFormation, EC2, S3, and the Code suite of services. Key concepts I learnt include Infrastructure as Code, how to use CloudFormation's IaC Generator, resolving circular dependences and resources that might depend on other resources to be created first. I also learned about using parameters to ensure some parameters are not hard coded to a template. This ensures the reusability of the template.

### Project reflection

This project took me approximately 3 hours The most challenging part was editing the CloudFormation template, which included troubleshooting for errors linked to cross-region resources, policy and role circular dependencies. It was most rewarding to deploy the template and even see them live in my account.

This project is part six of a series of DevOps projects where I'm building a CI/CD pipeline!

---

## Generating a CloudFormation Template

The IaC Generator is a tool in the CloudFormation console that helps us with writing CloudFormation templates faster. It works in a three-step process where it first scans the resources, lets us create a template based on the resources its found, and finally helps launch the template to deploy the resources in the template.

A CloudFormation template is text file that defines resources I want to deploy inside. The resources that I did add to my template include CodeArtifact domains and repositories, CodeDeploy Application, IAM Roles and Policies and S3 Bucket.

The resources I couldn’t add to my template were CodeBuild Projects and CodeDeploy deployment group because both resources are complicated by nature and require a lot of settings to configure. The IaC generator is no yet capable of scanning these settings so you'll have to write these manually.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_0495b046)

---

## Template Testing

Before testing my template, I deleted the existing resources from my account. This step is important because I do not want template deployment to fail since the resources share the same name.

I tested my template by launching a stack using the template I generated. The result of my first test was create failed because my IAM policy tried to an IAM role that didn't exist yet. This happens because the role is being created in the same template so it wasn't yet ready by the time the policy wanted to attach to it.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_f56730fd)

---

## DependsOn

To resolve the error, I opened up my template in a code editor to update the template manually. The DependsOn attribute means a resource needs to wait for another resource in the same template to be created first.

The DependsOn line was added to four different parts of my template: all four policies for CodeBuild which are policies that allow access to CodeArtifact, CodeConnection, CloudWatch and EC2 instances which is under CodeBuilds base policy. For CodeArtifact policy, I had to define two DependsOn line: the CodeBuild service role and EC2 instance role.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_f0df8018)

---

## Circular Dependencies

I gave my CloudFormation template another test! But this time there was a new error - circular dependencies. This error tells me that CloudFormation doesn't know what resource to deploy first. As it turns out, the policies depend on the role but the roles need the policies t be created. This puts CloudFormation in a loop making the template unusable.

To fix this error, I updated the template again by trying to understand why both roles and policies are referencing each other. Turns out, the roles have a section called `ManagePolicyArn` that references the policy they use. The policy definitions also have a depends on line that asked to wait for the roles to be created first. To resolve this loop, I deleted all the `ManagePolicyArn` sections in the template meaning the roles do not need to wait for the policies to be created.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_e6fd85ed)

---

## Manual Additions

In a project extension, I manually defined two more resources: a CodeBuild project and a CodeDeploy deployment group.

I also had to make sure the references were consistent in this template, so I edited the values for the S3 bucket ID, CodeDeploy Application ID and Service Role IDs to match the ID of those resources in the template.

I also introduced Parameters, which are like form fields in a CloudFormation template. Instead of hard coding a value inside a template, I get the CloudFormation stack to ask the user to fill in the value of x, y, z when they launch the resources. In this example, I created parameters for my github account information, i.e. account name and repo name.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_1cee0428)

---

## Success!

I could verify all the deployed resources by visiting the hyperlinks provided for each deployed resources. All the resources defined in the template were available. From IAM policies to the manually defined CodeBuild project!

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

---
