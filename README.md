# ECS-Fargate-CICD-GitHub-Actions

This project involved setting up an automated containerized application on Amazon ECS Fargate using Terraform, with Docker images pushed to Amazon ECR. I implemented secure private networking, Blue-Green deployments, and encrypted logging via CloudWatch and KMS.

## Project Highlights
- Provision ECS Fargate tasks within private subnets using Terraform to enhance security and isolation.
- Configure VPC Interface Endpoints for Amazon ECR and Amazon S3 to ensure all data traffic remains within the AWS network, reducing exposure to the public internet.
- Build and containerize the application using Docker, and securely push the image to Amazon Elastic Container Registry (ECR).
- Deploy the application to Amazon ECS services , with integrated health checks and enable centralized monitoring through Amazon CloudWatch Logs.
- Implement AWS Key Management Service (KMS) encryption , to secure CloudWatch log data and ECR image layers at rest.
- Integrate AWS CodeDeploy to orchestrate Blue-Green deployments , on ECS Fargate, facilitating seamless and controlled traffic shifting between application environments.
- Leverage ECS Fargate’s serverless container platform , to eliminate infrastructure management overhead and optimize costs through a pay-per-use pricing model.

## Architecture
### Target technology stack 
- **Terraform** to provision ECS Fargate tasks in **private subnets**.
- **VPC Endpoints** (ECR & S3) to keep traffic **within AWS network boundaries**.
- **Amazon ECR** Docker Image Build → Scan → Push to **Amazon ECR**.
- **AWS CloudWatch** App Deployment to ECS Services with **Health Checks + CloudWatch Logs**.
- **KMS encryption** for both CloudWatch logs and ECR image layers.
- **AWS Code Deploy** **Blue-Green Deployments**  using **CodeDeploy + GitHub Actions**.

### Blue/Green Deployment Strategy:
- Enhances availability by deploying updates to a separate (green) environment first.
- Enables safe testing and minimizes rollback risks and downtime.
- Improves reliability: Blue/Green deployments provide stable updates with built-in safety features.
  
### VPC Endpoints - Security First::
- Used Interface Endpoints for ECR and S3, ensuring that traffic stays within AWS's private network and has no exposure to the public internet.
- Deploying worker nodes in auto-scaling groups across private subnets.
- IAM role assignments to securely manage cluster access.
  
### Target architecture
![Architecture Diagram](/ecs_fargate_architecture.jpeg "Architecture Diagram")

## Key Learnings
✔ Leveraging VPC endpoints ensures all data stays within the AWS network — no internet exposure.
✔ Blue-Green deployments with CodeDeploy help achieve zero-downtime deployments.
✔ Using KMS encryption ensures data security at rest and in transit.
✔ Infrastructure as Code with Terraform makes the whole process repeatable and scalable.
✔ CloudWatch Container Insights offers real-time monitoring for proactive issue resolution.
✔ CI/CD integration with GitHub Actions simplifies automation and boosts productivity.

## Conclusion
This project was a great blend of DevOps best practices, security controls, and AWS-native automation.


