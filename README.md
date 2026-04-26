# AWS Secure Auto Scaling Architecture (HTTPS + ALB + ASG)

## Project Overview
This project demonstrates a production-style AWS architecture with secure (HTTPS), scalable, and highly available web deployment using an Application Load Balancer,  Auto Scaling Group across multiple Availability Zones, SSL/TLS and DNS

The system is designed to automatically provision, distribute, and manage traffic while serving content securely over HTTPS.

---

## Architecture Flow
Internet → HTTPS (ALB) → Auto Scaling Group → EC2 Instances

---

## AWS Services Used
- Amazon VPC
- Public Subnets (Multi-AZ)
- Internet Gateway
- Application Load Balancer (ALB)
- Target Groups
- Auto Scaling Group (ASG)
- EC2 Instances
- AWS Certificate Manager (ACM)
- Route 53 (DNS)
- Security Groups

---

## Key Features
- HTTPS enabled using SSL/TLS certificate
- Traffic distribution via Application Load Balancer
- Auto Scaling Group for self-healing and scalability
- Multi-AZ deployment for high availability
- Secure architecture with controlled inbound traffic

---

## Deployment Steps

1. Created VPC with CIDR 10.0.0.0/16  
2. Configured public subnets across multiple Availability Zones  
3. Attached Internet Gateway and configured routing  
4. Created Application Load Balancer and target group  
5. Created Launch Template with automated Nginx setup  
6. Deployed Auto Scaling Group across multiple AZs
7. Registered a domain and hosted it on Route 53
8. Requested and validated SSL certificate using ACM  
9. Configured HTTPS listener (port 443) on ALB  
10. Implemented HTTP to HTTPS redirection 

---
## What Went Wrong 
While updating the user data in the launch template, I used an old template version with a different security group as the source. This resulted in a 504 Gateway Timeout error.

## Fix
Created a new version of the template, and made the new launch template with the right security group as the source template.

## Validation & Testing

- Verified HTTPS access via domain name  
- Confirmed browser security (SSL/TLS lock)  
- Tested Auto Scaling behavior by terminating instances – ASG replaced them automatically after 5 mins. However, while the new one was provisioning, ALB redirected incoming traffic to the second server. That's Auto Scaling working as designed
- Verified continuous availability through ALB  

---

## Take Home

- HTTPS requires a domain, SSL certificate, and proper ALB configuration  
- Application Load Balancer handles SSL termination, not EC2 instances  
- DNS and certificate propagation can cause temporary inconsistencies across browsers  
- Security group inbound rules must be correctly configured for traffic flow  
- Validating infrastructure before automation prevents scaling misconfigurations  

---

## Next

I will add Monitoring , Alerts and  Dynamic Scaling. This will allow the architecture to respond to real traffic patterns automatically rather than relying on manual configuration.

---

## Screenshots
1. Final Website (HTTPS)
<img width="2880" height="1800" alt="1 Final Website" src="https://github.com/user-attachments/assets/2505750b-85e0-49cd-9622-b8688e866ca0" />

2. Architecture Diagram
<img width="2880" height="1800" alt="2 Architecture Diagram" src="https://github.com/user-attachments/assets/d1f27e71-f9e7-4330-8fd8-e5e7132ced6a" />

3. ALB Listeners
<img width="2880" height="1800" alt="3 ALB listeners" src="https://github.com/user-attachments/assets/c9cc0b88-0d4d-49b4-ba3f-32c47464d8cf" />

4. Target Group
<img width="2880" height="1800" alt="4 Target Group" src="https://github.com/user-attachments/assets/f472e632-aaa6-4301-b8b4-65d6d6874419" />

5. EC2 instances by Auto Scaling Group
<img width="2880" height="1800" alt="5 ASG Running Instances" src="https://github.com/user-attachments/assets/142a0af6-e493-46ef-9185-979dbdc5cd64" />

6. ACM Certificate
<img width="2880" height="1800" alt="6 ACM Certificate" src="https://github.com/user-attachments/assets/a91efd27-e047-45cc-b017-d487712cd9fd" />

7. Route 53
<img width="2880" height="1800" alt="7 Route 53" src="https://github.com/user-attachments/assets/eb65c740-24e9-4a9a-bdac-f1dad79c0a56" />

---

## Note

This architecture is designed for learning and portfolio purposes. In production, further enhancements would include:

- Private subnets for EC2 instances  
- NAT Gateway for outbound traffic  
- Monitoring and alerting using CloudWatch
