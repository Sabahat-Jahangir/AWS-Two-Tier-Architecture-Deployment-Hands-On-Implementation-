# AWS Two-Tier Architecture Deployment (Hands-On Implementation)

## Overview

This repository documents my complete hands-on implementation of a **Two-Tier Architecture on AWS**, starting from basic network design to deploying a fully functional web application.

The goal of this project was not just to understand cloud concepts, but to **implement them step-by-step in a real environment**, following practical industry practices.

To make this reproducible for others, I created a **detailed 100+ slide PowerPoint guide**, where each step is explained with:

* Screenshots
* Commands
* Configurations
* Reasoning behind decisions

This repository acts as a structured summary of that implementation.

---

## What This Project Covers

This project demonstrates how to design and deploy a **secure and scalable cloud architecture** using AWS services.

### Core Concepts Implemented

* Virtual Private Cloud (VPC) design
* CIDR-based IP planning
* Public and Private Subnet architecture
* Internet Gateway configuration
* Route Tables (Public vs Private routing)
* Network ACLs (stateless filtering)
* Security Groups (stateful firewall rules)
* EC2 instance deployment and configuration
* SSH secure access
* Web server setup using Apache
* WordPress deployment on EC2
* Amazon RDS (MySQL) setup in private subnet
* Secure communication between EC2 and RDS

---

## Architecture Explanation (Simple Flow)

This is a classic **Two-Tier Architecture**, divided into:

### 1. Presentation Layer (Public Tier)

* Hosted on **EC2 (Amazon Linux 2)**
* Accessible via the internet
* Runs:

  * Apache Web Server
  * PHP
  * WordPress

### 2. Data Layer (Private Tier)

* Hosted on **Amazon RDS (MySQL)**
* Placed inside a **private subnet**
* Not accessible from the internet

### Communication Flow

1. User sends request through browser
2. Request reaches EC2 instance (Public Subnet)
3. WordPress application processes the request
4. EC2 communicates securely with RDS (Private Subnet)
5. Database sends response back to EC2
6. EC2 returns final response to user

This ensures:

* Public access only to the web layer
* Database remains isolated and protected

---

## Detailed Implementation Breakdown

### Step 1: VPC Design and Planning

* Defined CIDR block: `10.0.0.0/16`
* Divided into subnets:

  * Public Subnet 1 & 2
  * Private Subnet 1 & 2

This allows:

* Proper segmentation
* Future scalability
* Multi-AZ design readiness

---

### Step 2: Subnet Configuration

* Public Subnets:

  * Used for EC2 instances
  * Allow internet access

* Private Subnets:

  * Used for RDS
  * No direct internet access

---

### Step 3: Internet Connectivity

* Created and attached **Internet Gateway (IGW)** to VPC
* Configured **Route Table**:

  * Public Subnet → IGW (0.0.0.0/0)
  * Private Subnet → No IGW

---

### Step 4: Security Implementation

#### Network ACLs

* Configured inbound and outbound rules
* Stateless filtering

#### Security Groups

**Public EC2 Security Group**

* SSH (Port 22) → Restricted to my IP
* HTTP (Port 80) → Open to internet

**Private RDS Security Group**

* MySQL (Port 3306) → Only accessible from EC2

This ensures:

* Controlled access
* No direct database exposure

---

### Step 5: EC2 Deployment

* Launched **Amazon Linux 2 instance**
* Instance type: `t3.micro`
* Enabled:

  * Public IP assignment
  * SSH access via key pair

---

### Step 6: Web Server Setup

Installed and configured:

* Apache (httpd)
* PHP
* MySQL client

Commands used:

* System updates
* Package installations
* Service management

Verified server using:

* Browser access via public IP

---

### Step 7: WordPress Deployment

* Downloaded latest WordPress package
* Extracted files into web directory
* Configured permissions
* Linked WordPress with database

Final verification:

* Accessed `/wp-admin`
* Completed setup wizard
* Created test content

---

### Step 8: RDS (Database Layer)

* Engine: MySQL
* Instance type: db.t3.micro
* Storage: 20GB

Key configurations:

* Placed in **private subnet**
* Disabled public access
* Created database and user
* Connected EC2 to RDS using endpoint

---

### Step 9: Secure EC2 ↔ RDS Communication

* Configured Security Groups:

  * Allowed MySQL traffic only from EC2 SG
* Used environment variables and config files
* Verified connection using MySQL commands

---

## What Makes This Project Valuable

This is not just a theoretical setup.

* Every component was deployed manually
* Every configuration was tested
* Every step was documented with screenshots

The 100+ slide PPT includes:

* Exact AWS console steps
* CLI commands
* Common errors and fixes
* Clear explanation of why each step is done

This makes it highly useful for:

* Beginners in AWS
* Students preparing for cloud roles
* Anyone struggling with real deployment

---

## Challenges Faced

* Understanding CIDR and subnet planning
* Configuring correct security group rules
* Establishing EC2 to RDS connectivity
* Debugging WordPress database connection issues

These challenges helped in developing a deeper understanding of:

* Networking
* Cloud security
* System configuration

---

## Key Learnings

* Cloud architecture is not just about services, it is about **design decisions**
* Security must be implemented at multiple layers
* Misconfigured rules can break the entire system
* Practical implementation teaches more than theory

---

## Repository Contents

* README.md (this file)
* Deployment PPT (step-by-step guide)
* Screenshots (if included)

---

## How to Use This Project

1. Go through the README for understanding
2. Follow the PPT step-by-step
3. Recreate the architecture in your AWS account
4. Test each layer individually

---

## Future Improvements

* Add Load Balancer (for scalability)
* Use Auto Scaling Group
* Implement HTTPS with SSL
* Add monitoring using CloudWatch
* Automate deployment using Terraform

---

## Conclusion

This project represents a complete hands-on journey of building a cloud architecture from scratch. It reflects not only technical understanding but also the ability to implement, troubleshoot, and document a real-world system.
