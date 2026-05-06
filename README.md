# AWS Static Website Hosting (EC2 + Custom VPC)

## Overview
In this project, I deployed a static website on Amazon Web Services using Amazon EC2 within a custom Amazon VPC. I built it from the ground up to understand how the underlying infrastructure actually works - networking, routing, access control, and how a server ultimately delivers content to a user.

## Architecture

![Architecture](images/architecture.png)

The setup includes:
- Custom VPC (`My-Website-VPC`)
- Public Subnet (`My-Website-Subnet`)
- Internet Gateway (`My-Website-Internet-Gateway`)
- Route Table (`My-Website-Route-Table`)
- Network ACL (`My-Website-NACL`)
- Security Group (`My-Website-SG`)
- EC2 Instance (`My-Ubuntu-Instance`)
- Nginx Web Server

## Infrastructure Setup

### VPC and Subnet
A custom VPC was created with CIDR `192.168.0.0/16`, along with a public subnet (`192.168.0.0/24`) to host the EC2 instance.

![VPC Setup](images/vpc.png)

### Routing and Internet Access
An Internet Gateway was attached, and a route (`0.0.0.0/0`) was configured to allow internet traffic.

![Route Table](images/route-table.png)

![Internet Gateway](images/internet-gateway.png)

### Security Configuration
Security Group was setup and configured. SSH (Port 22) restricted to my IP. HTTP (Port 80) open to public.

![Security Group](images/security-group.png)

### Network ACL
NACL was setup and Rule 110 was configured to allow HTTP (Port 80). Rule 120 was configured to allow SSH (Port 22) from my IP. This added subnet-level traffic control on top of the security group.

![NACL](images/nacl.png)

### EC2 Deployment
An Ubuntu-based EC2 instance (`My-Ubuntu-Instance`) was launched and configured.

![EC2](images/ec2.png)

### Server Preparation & Nginx
Nginx was used to serve the static website. But before then I prepard the server via the following:

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
sudo apt install nginx -y
```

![Nginx](images/nginx.png)

### Website Deployment
The website files were deployed to `/var/www/html` accessible via `http://13.62.46.120`

![Website](images/website.png)

### Domain Configuration
The EC2 public IP was mapped to my domain `victoredet.online`. This made the deployment more realistic and user-friendly.

![Domain](images/domain.png)

### Monitoring & Logging
To observe network activity, I enabled VPC Flow Logs. `Accept-Event-Log` recorded allowed traffic while `Reject-Event-Log` recorded denied traffic, all stored in S3 bucket named `datawarehouse-flowlog`. This provided visibility into how traffic was flowing through the environment.

![Flow Logs](images/flowlogs.png)

### Validation
I verified the setup by checking Nginx service status, accessing the site via IP and domain, confirming security rules (SSH & HTTP), and validating flow logs in S3.

## Key Takeaways: 
This project helped me understand how traffic flows inside a VPC, the difference between Security Groups and NACLs, EC2 as a raw compute environment, how web servers deliver content, basic observability using VPC Flow Logs, and the importance of debugging infrastructure.

## Limitations
- This is a single EC2 instance (no scaling or redundancy)
- It was a manual setup (no Infrastructure as Code yet)
- It is not optimized for static hosting (EC2 vs object storage)
- Monitoring is basic (flow logs only)

## Future Improvements
Going forward, I will: 
- Move to object storage & CDN for static hosting
- Introduce Terraform
- Add load balancing and auto-scaling
- Implement centralized monitoring and alerting, and
- Strengthen security with WAF and IAM roles

##  Management
After validating the deployment, I deleted all resources (including the VPC and EC2 instance) to avoid ongoing costs.

## Status
✔ Deployed
✔ Tested
✔ Validated
✔ Cleaned up (cost-optimized teardown)
