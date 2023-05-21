# Static Website Hosting Solution

This repo is a solution for AWS S3 website hosting through Codepipeline and CloudFormation.

## Use of the Repo

This is a simple example solution of how to deploy a static website on AWS S3 Bucket by configuring AWS CodePipeline and CloudFormation stack. The Source code is stored in Github repo. Once the code is changed, AWS CodePipeline(CI/CD Pipeline) will be trigger automatically to deploy the code to S3 Bucket for the latest version of Website App. All the resources are created by CloudFormation for easy deployment and maintenance(Infrastructure as code).

### Requirement

- AWS Account
- Github Account

### Step one:

- Fork the repo into your own Github account and clone it to your local environment.
- To create a GitHub personal access token and then update the pipeline structure with the new token:
  - In GitHub, from the drop-down option on your profile photo, choose Settings.
  - Choose Developer settings, and then choose Personal access tokens.
  - Choose Generate new token. The New personal access token page appears.
  - Under Select scopes, select admin:repo_hook and repo.
  - Choose Generate token.
  - Next to the generated token, choose the copy icon. And copy the token into your text editor for late use.

### Step two:

- Login your AWS Console with the user which have sufficient privilege(for simple, we use the user with admin privilege)
- Go to CloudFormation console, and click "Create Stack"
- In Prerequisite choose:

  - Prepare template: Template is ready
  - Template source: Upload a template file
  - Choose File: Choose the "s3-website.yml" file in the previous repo
  - Choose your stack name
  - Input Parameters:

    - WebsiteBucketName:
      Enter a unique name for the S3 bucket used for website hosting
    - ArtifactBucketName:
      Enter a unique name for the S3 bucket used for artifact storage
    - GitHubRepositoryName:
      Enter the name of the public GitHub repository
    - GitHubOwner:
      Enter the GitHub repository owner's username
    - OAuthToken:
      Enter the GitHub personal access token

  - Create your CloudFormation stack. The CloudFormation stack creates:
    - Two S3 Buckets: one for website hosting with relevant bucket policy. One for Artifact Store.
    - Pipeline role with AWSCodePipeline_FullAccess and AmazonS3FullAccess for resource creation
    - AWS CodePipeline is configured with source action and deploy action. Once the new code is merged into main branch, CodePipeline will trigger automatically to deploy the latest version of website.
    - Output: The website url is print out for use.

## Improvements and Additional Work(If enough time)

In Real World, we prefer to build multi-tier infrastructure to deploy our Web Application (Three-Tiers for example):

- Route traffic from web client based on the request path for static and dynamic contents using Amazon Route 53.
- Protect your web application from common web exploits with a web application firewall like AWS WAF.
- Use a content delivery network (CDN), like Amazon CloudFront, to reduce latency of delivering your static.
- Use Amazon Simple Storage Service (Amazon S3) to store static contents and backups.
- Simplify your SSL certificates management using ACM.
- Use an internet-facing Application Load Balancer to distribute web traffic to your web servers spread across multiple availability zones.
- Use NAT gateways in each public subnet enable Amazon Elastic Compute Cloud (Amazon EC2) instances in private subnets to access the internet.
- Use an internal Application Load Balancer to distribute traffic to your application servers spread across multiple Availability Zones.
- Simplify your database administration by running your database layer in Amazon Relational Database Service (Amazon RDS).
- If database access patterns are read- heavy, consider taking advantage of a caching layer like Amazon ElastiCache.
- Consider using a shared storage service, like Amazon Elastic File System (Amazon EFS), if your servers have access to shared files.
- Monitoring with Prometheus and Grafana(or CloudWatch), and Logging with ELK server(or CloudWatch).
- Deploy web application in different AZs and regions, and configure auto-scaling for high availability and scalability.
- Configure disaster recovery for both web application and database, and backup database regularly.

## Alternative Solutions:

- For CI/CD Pipeline: - We can Configure Jenkins Server or CircleCI, TeamCity instead.
- For Infrastructure as code: - We can configure Terraform for Infrastructure Building and Ansible for Infrastructure Configuration.
- For Compute: - We can build with EC2, Docker Container in EC2, ECS (Fargate or EC2), EKS or Serverless web application(S3+ API Gateway + Lambda + RDS/DynamoDB).
- For Credential Security: - We can use Vault or AWS Secret Manager to store and retrieve credentials safely.
- For Network Security: - We can configure VPC, Subnet, Load Balancer, WAF, CloudFront, AWS Shield for robust network security.
