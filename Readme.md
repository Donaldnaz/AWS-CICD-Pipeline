# AWS CI/CD Pipeline

This project demonstrates a **fully automated CI/CD pipeline** using **AWS native services** to deploy a **multi-tier Java application** (Profile App).  
It replaces manual Jenkins-based deployments with a **modern, serverless, AWS-native pipeline** that’s faster, scalable, and cost-efficient.

- **AWS CI/CD Pipeline** → [`aws-ci`](https://anasiezeikenna.notion.site/AWS-CI-CD-Pipeline-27005c74585e805c842dd5220424a735)  


### Problem Statement

Traditional Jenkins & EC2 deployments were:
- Manual, error-prone, and not scalable  
- Required constant maintenance of build servers and environments  
- Slowed down release cycles and increased operational costs  

> 🎯 **Goal:** Build a cloud-native CI/CD pipeline using AWS managed services — from commit to deployment — with zero manual steps.

---

### Tech Stack

| Layer | Service | Purpose |
|-------|----------|----------|
| Source Control | Bitbucket | Git repository for application source |
| CI/CD Orchestration | AWS CodePipeline | Automates build and deployment stages |
| Build Tool | AWS CodeBuild | Compiles and packages the Java WAR artifact |
| Deployment | AWS Elastic Beanstalk | Manages app server, scaling, and load balancing |
| Artifact Storage | Amazon S3 | Stores packaged build artifacts |
| Database | Amazon RDS (MySQL) | Backend database |
| DNS + CDN | Route 53 + CloudFront | Domain routing and global content delivery |
| Monitoring | CloudWatch | Centralized logging, metrics, and alarms |


### Architecture Overview

<img width="641" height="443" alt="image" src="https://github.com/user-attachments/assets/7cd667dd-e0ed-4f27-b2cc-e681e18bc596" />

**Pipeline Flow:**
1. Developer pushes code to **Bitbucket**  
2. **CodePipeline** triggers automatically  
3. **CodeBuild** compiles and packages WAR → uploads to **S3**  
4. **Elastic Beanstalk** deploys the new version automatically  
5. **CloudFront + Route 53** provide secure and global access  

---

### Implementation Steps

1. **Migrate source code** from GitHub → Bitbucket  
2. **Create Elastic Beanstalk environment** (Tomcat platform)  
3. **Provision Amazon RDS** (MySQL) for persistent storage  
4. **Define `buildspec.yml`** in Bitbucket repo for CodeBuild instructions  
5. **Create S3 bucket** for artifact storage  
6. **Set up AWS CodePipeline**:
   - Source: Bitbucket  
   - Build: CodeBuild  
   - Deploy: Elastic Beanstalk  
7. **Integrate Route 53 & CloudFront** for DNS + CDN  
8. **Push code to Bitbucket** and observe full pipeline automation  

---

### Setup Details

#### 1. Database
Created **Amazon RDS (MySQL)** instance and initialized schema via MySQL scripts.

#### 2. Application Platform
Deployed the app using **Elastic Beanstalk** (Tomcat 8.x environment).

#### 3. Networking & Security
- Configured **Security Groups** for RDS and Beanstalk.  
- Allowed inbound ports 80/443 for Beanstalk and 3306 for DB access.  

#### 4. CodeBuild
Configured build environment via `buildspec.yml`:

```yaml
version: 0.2
phases:
  install:
    commands:
      - echo Installing dependencies...
  build:
    commands:
      - echo Building project...
      - mvn clean package
artifacts:
  files:
    - target/*.war
