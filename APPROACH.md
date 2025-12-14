# Approach

## Objective

The objective of this assignment was to design and implement an end-to-end DevOps workflow which demonstrates real-world practices such as infrastructure provisioning, CI/CD automation, monitoring, logging, security, and state management using AWS, Terraform, Docker, and GitHub Actions.

The application logic is simple and a basic backend code is being used, because the primary focus is on DevOps principles rather than application development.

---

## Strategy

The overall approach is divided into following parts :

1. Selection of ready-made open-source application
2. Deploy frontend using Vercel
3. Provision backend infrastructure using Terraform
4. Containerize and deploy backend services
5. Implement CI/CD automation
6. Add monitoring, logging, and dashboards
7. Apply best practices for security and state management

---

## Application Selection

A ready-made Node.js backend application using PostgreSQL was chosen to be choosen to  avoid spending time on application logic ,but to be honest I found it very difficult to choose any application from internet and start working on it. So I created a very simple backend application with postgress and started working on it.

---

## Frontend Deployment by Vercel

The frontend was deployed on Vercel static/frontend hosting.
I choose front code from internet , fork it and then deployed it in Vercel.

Github front-end code : https://github.com/TriptiP-Code/material-dashboard-react
Front end url: https://material-dashboard-react-iota.vercel.app/dashboard

---

## Infrastructure with Terraform

Terraform was used as the Infrastructure as Code (IaC) tool to create AWS resources.

### Resources created:

* VPC with public and private subnets
  <img width="1918" height="862" alt="image" src="https://github.com/user-attachments/assets/676267f2-336d-4f71-a5a8-aae9245a052a" />

* Internet Gateway and routing
* EC2 instance for backend application
  
* Application Load Balancer (ALB)
  <img width="1873" height="812" alt="image" src="https://github.com/user-attachments/assets/c52f3cdd-a520-49f5-9b72-86125bec04d2" />

* Amazon RDS (PostgreSQL) in private subnet
  <img width="1915" height="903" alt="image" src="https://github.com/user-attachments/assets/1970da31-b02a-433b-8496-29bcbe0955ce" />

* Security Groups with least-privilege rules
  <img width="1901" height="522" alt="image" src="https://github.com/user-attachments/assets/0aa209d2-c2f9-49e4-a2ea-04073a3561e2" />


### Design:

* Backend EC2 placed in public subnet behind ALB
* RDS placed in private subnet for security

---

## State Management

Remote state management implemented using S3

* It helps in preventing state loss
* Enables safe CI/CD execution
* Avoids accidental resource recreation

State locking is enabled, ensuring infrastructure consistency.

---

## Containerization Strategy

The backend application is containerized using Docker.

* Docker image built via GitHub Actions
* Image pushed to Docker hub registry
* SSH to ec2 instance and run the application

---

## Deployment Automation (CI/CD)

GitHub Actions is used to implement CI/CD pipeline.

### CI (Continuous Integration):

* Gets triggered on Pull Request creation
  So I created a pr-check.yml which runs when a Pull Request is created by anyone.
  To test if it is properly running, I created a branch called test-pr-branch , and created test.txt file in the test-pr-branch. Raised a pull request.
  So according to logic 1st the pr-check.yml started and checked the code changes and once the test was passed then merge request was completed
  
* Runs automated tests
  <img width="1919" height="925" alt="image" src="https://github.com/user-attachments/assets/a52a2d4f-5571-462a-9cde-2077bd6442c4" />

* Prevents broken code from merging into main branch
* Gets merged successfully
  <img width="1919" height="1014" alt="image" src="https://github.com/user-attachments/assets/0724d9af-1254-4e9e-a273-4362556ad93b" />


### CD (Continuous Deployment):

* When any new code logic is pushed it builds and pushes Docker image
* Once docker imaged is pushed it ssh in ec2 instances and then pulls image from docker hub and runs the application and pipeline is successful
  <img width="1913" height="968" alt="image" src="https://github.com/user-attachments/assets/5711ca3b-a61a-46c1-ae20-ed533335ed95" />

* Application up and running , we can access app through Load Balancer
  <img width="1916" height="1017" alt="image" src="https://github.com/user-attachments/assets/42d2ca18-4cc3-4aca-9e4d-fd875982dc36" />


   
---

## Monitoring and Logging

Monitoring and logging were implemented using AWS CloudWatch.

### Monitoring:

* EC2 CPU and network metrics
* ALB request count and error metrics
* RDS database metrics

Two dashboards were created:

1. Infrastructure Dashboard
   <img width="1919" height="997" alt="image" src="https://github.com/user-attachments/assets/b7b6fc7b-9ee4-491c-ba98-5e98bb75acc8" />

2. Application Dashboard
   <img width="1919" height="928" alt="image" src="https://github.com/user-attachments/assets/c7ebe7ea-b7e8-465e-b396-852bea8b4e64" />


### Logging:

* Centralized log groups in CloudWatch
* EC2 system logs
* Application container logs

---

## Security Considerations

Security best practices are followed:

* RDS deployed in private subnet
* Security groups restrict traffic to required ports only
* Sensitive values passed via GitHub Secrets
* Public access limited via ALB

---

## Cost Optimization

Cost was kept minimal by:

* Using t3.micro EC2 instances
* Using free-tier eligible RDS instance

---

## Summary

This process demonstrates a practical, production-oriented DevOps workflow using industry-standard tools and practices. The solution emphasizes automation, security, observability, and reliability while keeping the system simple and maintainable.

The focus throughout the project was not just on making the system work, but on making it scalable, repeatable, and resilient.
