# Architecture-diagram

  ![manara-proj-2](https://github.com/user-attachments/assets/56a94755-c687-4dfc-981e-27faaed5c98e)


# 📘 **Highly Available and Secure Web Application Deployment on AWS – Architecture Documentation**

---

## 🚀 **1. Overview**

This architecture showcases a **resilient, scalable, and secure AWS cloud deployment** of a modern web application. It follows **AWS Well-Architected Framework principles**, enabling secure compute, efficient database access, and seamless autoscaling. The design leverages **multi-AZ redundancy**, **private/public subnet segregation**, and **AWS-managed services** like Secrets Manager for secret retrieval and RDS for data persistence.

---

## 🧱 **2. Core Components**

| Component                           | Description                                               |
| ----------------------------------- | --------------------------------------------------------- |
| **Amazon EC2**                      | Hosts application servers deployed in multiple AZs        |
| **Application Load Balancer (ALB)** | Distributes traffic to EC2 instances across AZs           |
| **Amazon RDS (MySQL/PostgreSQL)**   | Managed relational database residing in private subnets   |
| **AWS Secrets Manager**             | Securely manages and retrieves database credentials       |
| **Auto Scaling Group**              | Dynamically adjusts EC2 instance count based on demand    |
| **Internet Gateway**                | Enables communication between VPC and the public internet |

---

## 🌐 **3. Network Topology**

### 🔹 **Virtual Private Cloud (VPC)**

* A logically isolated network space.
* Hosts all resources in a secure environment.
* Contains **two Availability Zones (AZ1 & AZ2)** for **high availability**.

### 🔹 **Subnets**

| Subnet Type   | Availability Zone | Contents                                     |
| ------------- | ----------------- | -------------------------------------------- |
| **Public A**  | AZ1               | EC2 instance, ALB target                     |
| **Private A** | AZ1               | RDS instance                                 |
| **Public B**  | AZ2               | EC2 instance, ALB target                     |
| **Private B** | AZ2               | Reserved for failover, scalable architecture |

Each public subnet has a route to the **Internet Gateway**, while private subnets are isolated from the public internet for database security.

---

## ⚙️ **4. Application Flow**

### 👨‍💻 **Client Interaction**

1. A client initiates a request to the **Application Load Balancer (ALB)** using its DNS.
2. The ALB routes the request to one of the EC2 instances in **Public Subnet A or B** based on:

   * Load
   * Health status
   * Routing policies

### ⚙️ **Internal Flow**

3. The **EC2 instance** processes the request and securely fetches:

   * Database credentials from **AWS Secrets Manager**
   * Data from **Amazon RDS** in the **Private Subnet**
4. The application returns the response to the client via the ALB.

---

## 🔐 **5. Security Design**

| Layer                  | Measure                                                               |
| ---------------------- | --------------------------------------------------------------------- |
| **Network Layer**      | Private subnets, security groups, NACLs                               |
| **Secrets Management** | Credentials stored in **AWS Secrets Manager**, accessed via IAM roles |
| **Compute Layer**      | IAM roles attached to EC2 with least privilege                        |
| **Database Layer**     | RDS only accessible from EC2 within the VPC                           |

* No public access to the database.
* All secrets are encrypted using **AWS KMS**.

---

## 📈 **6. Auto Scaling**

* The architecture includes **Auto Scaling Groups** for EC2.
* Instances scale in/out based on metrics like:

  * CPU utilization
  * Request count per target
* Ensures performance under load while optimizing costs.

---

## 💾 **7. Database Layer – Amazon RDS**

* RDS deployed in **Private Subnet A**.
* Optionally enabled **Multi-AZ for automatic failover** to Private Subnet B.
* Security Group permits access **only from EC2 instances** within the VPC.

---

## 🧠 **8. Secrets Management – AWS Secrets Manager**

* **Secure credential storage and rotation**.
* EC2 instance accesses secrets using IAM roles.
* Eliminates hardcoded credentials and enhances security posture.

---

## 🔄 **9. Internet Access & Load Balancing**

| Component                     | Role                                               |
| ----------------------------- | -------------------------------------------------- |
| **Internet Gateway**          | Allows public subnets to communicate with internet |
| **Application Load Balancer** | Routes client traffic intelligently to EC2 targets |
| **Health Checks**             | Ensures requests go to healthy instances only      |

---

## 🧰 **10. Use Cases**

* **Production web applications**
* **E-commerce platforms**
* **APIs requiring secure DB access**
* **Microservices deployed on EC2**

---

## ✅ **11. Benefits**

| Feature               | Value                                                    |
| --------------------- | -------------------------------------------------------- |
| **High Availability** | Multi-AZ deployment with autoscaling                     |
| **Security**          | Private networking + Secrets Manager + IAM               |
| **Scalability**       | On-demand autoscaling for dynamic workloads              |
| **Maintainability**   | Decoupled architecture for easier updates and monitoring |
| **Cloud-Native**      | Fully managed services with best practices from AWS      |

---

