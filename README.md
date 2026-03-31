Highly Available 3-Tier Web Application on AWS 
  
 Project Overview 
This project demonstrates the deployment of a highly available, scalable, and secure 3-tier web application on AWS. 
The architecture is designed to: 
•	Handle traffic spikes (auto scaling)  
•	Ensure high availability (multi-AZ)  
•	Provide secure access (HTTPS)  
•	Separate concerns using 3-tier design  
  
Objectives 
•	Deploy infrastructure across multiple Availability Zones  
•	Configure Auto Scaling Group (ASG)  
•	Use Application Load Balancer (ALB) for traffic distribution  
•	Store static content in S3  
•	Secure application using HTTPS  
•	Implement secure access using Bastion Host  
•	Enable EBS encryption using KMS  
 
Architecture Diagram (Explain in words) 
Flow: 
User → ALB (HTTPS) → Target Group → EC2 (ASG in Private Subnets) 
                                     ↓ 
                                   EBS 
                                     ↓ 
                                    S3 
                 → Access Private EC2 
 
 



Infrastructure Setup 
VPC Configuration 
•	Created VPC with CIDR: 10.0.0.0/16  

  <img width="940" height="424" alt="image" src="https://github.com/user-attachments/assets/c372f0ff-0662-4256-a6e9-f4d669d742f9" />

Subnets 
•	Public Subnets:  
o	10.0.1.0/24 (AZ-1a)  
o	10.0.2.0/24 (AZ-1b)  
•	Private Subnets:  
o	10.0.3.0/24 (AZ-1a)  
o	10.0.4.0/24 (AZ-1b)  
<img width="940" height="405" alt="image" src="https://github.com/user-attachments/assets/7b8ee0e4-1fe0-4113-ab62-ed8441a2b851" />

  
Route Tables 
•	Public Route Table 
o	Route: 0.0.0.0/0 → Internet Gateway  
o	Associated with Public Subnets  
•	Private Route Table 
o	Route: 0.0.0.0/0 → NAT Gateway  
o	Associated with Private Subnets  
  <img width="940" height="405" alt="image" src="https://github.com/user-attachments/assets/773bfed0-72a6-4bae-9493-88ae577b33c3" />

 Internet & NAT Setup 
•	Internet Gateway attached to VPC  
•	NAT Gateway created in Public Subnet  
  <img width="940" height="318" alt="image" src="https://github.com/user-attachments/assets/e2e2f077-de99-4a93-babb-096cf0ce768d" />

 Golden AMI 
•	Installed Apache/Nginx  
•	Deployed application code  
•	Created reusable AMI for consistency  
  <img width="940" height="388" alt="image" src="https://github.com/user-attachments/assets/3b3e9569-ae99-4260-8c0b-dd79f5bacf6b" />

 Launch Template 
•	Instance Type: t3.micro  
•	AMI: Golden AMI  
•	Security Group attached  
•	Configured User Data for automation  
  <img width="940" height="402" alt="image" src="https://github.com/user-attachments/assets/a394487b-9b0b-4323-b83a-9e60ce6d4194" />

Storage Layer 
 EBS Configuration 
•	Volume Type: gp3  
•	Size: 20 GB  
•	Encryption enabled using AWS Key Management Service  
  <img width="940" height="397" alt="image" src="https://github.com/user-attachments/assets/7d27a011-e7bc-4a5d-b99d-2d8e08a37a09" />

 S3 Configuration 
Used Amazon S3 for: 
•	Static files (images, CSS, JS)  
Features: 
•	Versioning enabled  
•	Highly durable storage  
  <img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/b5961013-c4e9-446d-bedc-9f204ca0005e" />

 
 Target Group 
•	Protocol: HTTP  
•	Port: 80  
•	Health checks enabled  
  <img width="940" height="300" alt="image" src="https://github.com/user-attachments/assets/27d93361-d0f4-4808-b0ce-cb833490b506" />

 Load Balancer Layer 
 Application Load Balancer (ALB) 
•	Type: Internet-facing  
•	Deployed in Public Subnets  
  <img width="940" height="403" alt="image" src="https://github.com/user-attachments/assets/598f8c33-485b-4656-9198-71f2920bacca" />

Auto Scaling Group (ASG) 
•	Min: 2 | Desired: 2 | Max: 4  
•	Deployed in Private Subnets across 2 AZs  
Scaling Policy: 
•	Target tracking based on CPU utilization (70%)  
Features: 
•	Self-healing (replaces unhealthy instances)  
•	High availability  
  <img width="940" height="423" alt="image" src="https://github.com/user-attachments/assets/ba5881d5-687b-4f23-b1bb-297133173954" />

 
HTTPS Configuration 
Used AWS Certificate Manager 
•	Listener: HTTPS (443)  
•	SSL certificate attached  
Advanced Features 
•	Cross-Zone Load Balancing enabled  
•	Sticky Sessions enabled (1 hour)  
Security Configuration 
Security Groups 
ALB SG 
•	Allow HTTPS (443) from Internet  
App Server SG 
•	Allow HTTP only from ALB SG  
Bastion SG 
•	Allow SSH (22) only from My IP  
 
Access Flow 
User → ALB → EC2 (Private) 
Admin → Bastion → EC2 
 
Validation & Testing 
•	Accessed application via ALB DNS  
                      
•	Verified load balancing across instances  
•	Tested auto scaling using load  
•	Terminated instance → ASG recreated it  
•	Verified HTTPS access  
 
 
 Key Concepts Used 
•	High Availability (Multi-AZ)  
•	Auto Scaling  
•	Load Balancing  
•	Secure Access (Bastion Host)  
•	Data Encryption (EBS + KMS)  
•	Object Storage (S3)  
•	HTTPS Security  
 
Challenges Faced 
(You can customize this section) 
Example: 
•	Incorrect route table association  
•	Security group misconfiguration  
•	Target group health check failures  
 
Conclusion 
The project successfully demonstrates a scalable, secure, and highly available AWS architecture capable of handling production-level workloads. 
It follows best practices like: 
•	Multi-AZ deployment  
•	Auto scaling  
•	Secure networking  
•	Encrypted storage  
 

