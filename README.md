# 📌 Cloud Infrastructure & Deployment with AWS ECS

## 🚨 Project Overview

![overview](resources/overview.png)

---

## 🏗 Architecture & Technologies

### 🧱 Infrastructure & Networking

- <img src="https://i.imgur.com/FNOLEI9.jpeg" width="30" height="30" /> **Virtual Private Cloud (VPC)**: A custom network setup with multiple subnets, including public, private, and database subnets, ensuring workload isolation and security.
- **Internet Gateway (IGW)**: Allows access to public resources from the internet, attached to the VPC.
- **NAT Gateway (NGW)**: Provides secure internet access for private subnets, associated with an Elastic IP.
- **Route Tables**:
  - Public route table directs traffic to the internet via IGW.
  - Private route table routes traffic via NGW.
- **Network ACLs (NACLs)**: Manages inbound and outbound traffic across subnets to enhance security.
- **Security Groups (SGs)**:
  - Public security group allows HTTP, HTTPS, and application-specific traffic.
  - Private security group restricts access, permitting only traffic from the load balancer.

![vpc](resources/vpc.png)

---

### ⚖️ Load Balancing & DNS

- <img src="https://i.imgur.com/GMr01eh.png" width="30" height="30" /> **Application Load Balancer (ALB)**: Manages HTTP/HTTPS traffic and distributes it to services based on routing rules.

![alb](resources/alb.png)

- <img src="https://i.imgur.com/jFcQvRW.png" width="30" height="30" /> **Web Application Firewall (WAF)**: Protects against common security threats, including SQL Injection (SQLi) and Cross-Site Scripting (XSS).

  - DEV
    ![waf-dev](resources/waf-dev.png)
  - PROD
    ![waf-prod](resources/waf-prod.png)

- <img src="https://i.imgur.com/CHzMALx.png" width="30" height="30" /> **Amazon Route 53**: Routes domain traffic to the appropriate endpoints.

![route-53](resources/route-53.png)

- <img src="https://i.imgur.com/s1HtY0n.png" width="30" height="30" /> **Certificate Manager (ACM)**: Provides SSL/TLS certificates to ensure secure communication over HTTPS.

![acm](resources/acm.png)

---

### 🚀 Container Orchestration

- <img src="https://i.imgur.com/SWw2HAB.png" width="30" height="30" /> **Elastic Container Service (ACM)**: A fully managed container orchestration service that simplifies the deployment, scaling, and management of containerized applications.
- <img src="https://i.imgur.com/WZPqH1T.png" width="30" height="30" /> **Serverless Compute Engine (Fargate)**: Eliminates the need for server provisioning, allowing automatic scaling and resource optimization.

  - DEV
    ![dev-cluster](resources/dev-cluster.png)
  - PROD
    ![prod-cluster](resources/prod-cluster.png)

---

## ♻️ CI/CD Pipeline

This CI/CD pipeline is built using **AWS DevOps Services**, ensuring efficient and automated deployment. It leverages:

- <img src="https://i.imgur.com/4Ztfkdb.png" width="30" height="30" /> **AWS CodePipeline** for continuous integration and deployment automation.
- <img src="https://i.imgur.com/upNyo8b.png" width="30" height="30" /> **AWS CodeBuild** for compiling, packaging, and containerizing applications.
- <img src="https://i.imgur.com/MmcSYIE.png" width="30" height="30" /> **AWS CodeDeploy** for automated and zero-downtime deployments.
- <img src="https://i.imgur.com/EVxxE9V.png" width="30" height="30" /> **Amazon SNS** for deployment approval notifications and alerts.

![pipeline](resources/pipeline.png)

- <img src="https://i.imgur.com/4rPiNGc.png" width="30" height="30" /> **Amazon ECR** for secure container image storage.
- <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original.svg" alt="Docker" width="30" height="30"/> **Docker** for containerization and efficient deployment.

![ecr](resources/ecr.png)

---

### ⚙️ Build & Deployment Stages

1. **Code Commit & Build**

- Developers push code to the **GitHub repository**.
- AWS **CodePipeline** detects the changes and triggers the **CodeBuild** process.
- The application is compiled, packaged, and containerized.
- The built image is pushed to **ECR** for deployment.

  ![build](resources/build.png)

2. **Development Deployment**

- The pipeline automatically deploys the new version to the **development environment** for testing whenever changes are pushed to the `dev` branch.

  ![pipeline-dev](resources/pipeline-dev.png)

3. **Merge & Trigger Production Pipeline**

- Once the changes in the `dev` branch are tested and approved, they are merged into the `main` branch.
- **AWS CodePipeline for production** is configured to **only trigger when there is a new commit in the `main` branch**.

  ![pipeline-prod](resources/pipeline-prod.png)

4. **Approval & Production Deployment**

- A manual approval step (e.g., via SNS) ensures only validated changes reach production.
- Once approved, the deployment proceeds to the production environment.

  ![sns-approve](resources/sns-approve.png)

---

## 🎉 Deployment Results

### 🛠 Dev Environment

- Changes pushed to the `dev` branch are automatically deployed to the **development environment**.
- This environment is used for testing and validating new features before they are merged into `main`.
- Developers can access the latest build to verify functionality, debug issues, and ensure stability.

  ![dev](resources/dev.png)

### ⚡ Before Merge

- The production environment before the latest update.
- This represents the last stable version running before new changes are deployed.

  ![prod-before](resources/prod-before.png)

### ✨ After Merge

- The production environment after the latest deployment.
- The newly merged and approved changes are now live.

  ![prod-after](resources/prod-after.png)

---

## 📡 Monitoring

### <img src="https://i.imgur.com/rAsQAY5.png" width="30" height="30" /> **CloudWatch**

- **Monitoring & Logging**: Collects and stores logs from ECS containers for application tracking.
- **Log Insights**: Enables querying logs for error analysis and performance monitoring.
- **Alarms & Alerts**: Triggers alerts based on application errors or resource overuse.

  - **Logs**  
    ![log-cloudwatch](resources/log-cloudwatch.png)

  - **Alarms**  
    ![alarm-cloudwatch](resources/alarm-cloudwatch.png)
