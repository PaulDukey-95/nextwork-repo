<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** Paul Ndukwe  
**Email:** paulnd95@gmail.com

---

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will demonstrate the use of CodePipeline to set up a CI/CD pipeline to automate the flow from GitHub to CodeDeploy! I will be able to simply push a change to my code and see the live update without touching CodeBuild or CodeDeploy.

### Key tools and concepts

Services I used were CodePipeline, CodeDeploy, CodeBuild, CodeArtifacts, GitHub, VS Code, EC2, S3, CloudFormation and IAM. Key concepts I learnt include the different stages in CI/CD pipeline, handling rollbacks and webhooks.

### Project reflection

This project took me approximately 3hrs. There were not any challenging part in this project. It was most rewarding to see the live, deployed web app update without having to build and deploy the project myself.



---

## Starting a CI/CD Pipeline

AWS CodePipeline is a service that helps me automate the process of running a software, i.e, moving code from GitHub (source repo) all the way to deployment (CodeDeploy). This makes sure deployments are consistent and reliable with less human errors.

CodePipeline offers different execution modes based on how we treat multiple runs of the same pipeline. I chose 'Superseded' which will focus on running the latest run and cancel older ones. Other options include Queued and Parallel.

A service role gets created automatically during setup so CodePipeline can access and interact with other AWS services like CodeBuild, CodeDeploy, and S3 on your behalf, enabling it to run the pipeline stages securely and efficiently.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

The three stages I've set up in my CI/CD pipeline are Source, Build, and Deploy. While setting up each part, I learnt about integrating GitHub for version control, using CodeBuild to compile and test code, and deploying reliably with CodeDeploy.




CodePipeline organizes the three stages into a single diagram that shows the flow of changes from source - build - deploy. In each stage, you can see more details on the pipeline execution it belongs to and shortcuts to the connected service.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

In the Source stage, the default branch tells CodePipeline exactly which version of my code I want to use. In prod. environments, you can have many different environment to represent different versions of your web app so specification is cruicial.

The source stage is also where you enable webhook events, which like notifications for whenever you make a change to the source code. It detects changes and alerts CodePipeline to trigger a new run.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

The Build stage sets up how I'll build my web app ready for deployment. I configured CodeBuild to be the build provider and use the input artifact from the source stage. The input artifact for the build stage is the compressed code from GitHub.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

The Deploy stage is where I set up CodeDeploy to be the deployment provider. It takes the build artifact from CodeBuild and the application and deployment settings defined in my deployment group.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered by changes to my GitHub, I tested my pipeline by adding a new line to my index.jsp file, commiting and pushing the changes from VS Code to GitHub.

The moment I pushed the code change CodePipeline responded immediately triggering a new build and deploy stage. The commit message under each stage reflects the latest code change. The source stage was the first stage to reflect my latest commit.

Once my pipeline executed successfully, I checked that GitHub, CodeBuild, and CodeDeploy worked without errors. I then opened the EC2 web app server’s public DNS in my browser to confirm the application was deployed and running correctly.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

---

---
