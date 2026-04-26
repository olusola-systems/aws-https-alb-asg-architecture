# AWS Secure Auto Scaling Architecture (HTTPS + ALB + ASG)

## Project Overview
This project demonstrates a production-style AWS architecture with secure (HTTPS), scalable, and highly available web deployment using an Application Load Balancer,  Auto Scaling Group across multiple Availability Zones, SSL/TLS and DNS

The system is designed to automatically provision, distribute, and manage traffic while serving content securely over HTTPS.

---

## Architecture Flow
<img width="2880" height="1800" alt="Architecture Diagram" src="https://github.com/user-attachments/assets/bdec2676-ca68-4ff1-8deb-4093b354e1a8" />

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

2. ALB Listeners
<img width="2880" height="1800" alt="ALB listeners" src="https://github.com/user-attachments/assets/fe0a6537-63ef-43b4-99e7-2da4aba50f32" />

3. Target Group
<img width="2880" height="1800" alt="Target Group" src="https://github.com/user-attachments/assets/54ef1e4e-6a58-4c6b-9817-c96fe77c6c3e" />

4. EC2 instances by Auto Scaling Group
<img width="2880" height="1800" alt="ASG Running Instances" src="https://github.com/user-attachments/assets/dbd21d8f-be89-4331-9aac-149d4b342625" />

5. ACM Certificate
<img width="2880" height="1800" alt="ACM Certificate" src="https://github.com/user-attachments/assets/a0f374c6-46e3-47ce-900f-d1402273dea4" />

6. Route 53
<img width="2880" height="1800" alt="Route 53" src="https://github.com/user-attachments/assets/5f78c045-cbc1-4e0c-99f2-b4abba9dd819" />

---

## Note

This architecture is designed for learning and portfolio purposes. In production, further enhancements would include:

- Private subnets for EC2 instances  
- NAT Gateway for outbound traffic  
- Monitoring and alerting using CloudWatch
