# VPC Flow Logs to S3 Setup

This project provides configuration files and instructions to set up VPC Flow Logs in AWS and store them in an S3 bucket using an IAM role with the proper permissions.

---

## 🎯 Goal

Enable VPC Flow Logs on a VPC to capture all network traffic (accepted and rejected) and store the logs securely in an S3 bucket for auditing and monitoring purposes.

---

## ✅ Step-by-Step Setup Guide

| Step                          | Description                                             |
|-------------------------------|---------------------------------------------------------|
| **1. Create S3 Bucket**       | Create a bucket `vpc-flow-logs-bucket` with public access disabled and attach a bucket policy. |
| **2. Create IAM Role**         | Create IAM Role `VPCFlowLogsToS3Role` with trust policy and permission policy to write to S3. |
| **3. Create VPC**             | Create a new VPC `MyAuditVPC` with CIDR block `10.0.0.0/16`. |
| **4. Enable VPC Flow Logs**   | Enable flow logs on the VPC to send all logs to the S3 bucket using the IAM role. |

---

### 1. Create S3 Bucket

**Goal:** Create a secure S3 bucket to store VPC Flow Logs.

**Steps:**

1. Go to **AWS S3 Console**
2. Click **Create bucket**
3. Set **Bucket name**: `vpc-flow-logs-bucket`
4. Disable **Block Public Access**
5. Click **Create bucket**

**Set Bucket Policy:**

- Go to **Permissions** tab → **Bucket policy**
- Paste the contents of `s3/bucket-policy.json`
- Click **Save**

---

### 2. Create IAM Role for VPC Flow Logs

**Goal:** Create an IAM role with permission for VPC Flow Logs to write logs to the S3 bucket.

**Steps:**

1. Open **IAM Console** → **Roles** → **Create role**
2. Choose **AWS Service** → **EC2**
3. Click **Next**
4. Paste the trust policy from `iam/role-trust-policy.json`
5. Name the role: `VPCFlowLogsToS3Role`
6. Click **Create role**

**Attach Permission Policy:**

1. Go to **Policies** → **Create policy**
2. Select **JSON** tab
3. Paste the content of `iam/vpc-flow-logs-s3-policy.json`
4. Name it `VPCFlowLogsS3PutPolicy`
5. Create and attach this policy to `VPCFlowLogsToS3Role`

---

### 3. Create VPC

**Goal:** Create a VPC where flow logs will be enabled.

**Steps:**

1. Open **VPC Console**
2. Click **Create VPC**
3. Choose **VPC only**
4. Enter:
   - **Name tag:** `MyAuditVPC`
   - **IPv4 CIDR block:** `10.0.0.0/16`
5. Click **Create VPC**

---

### 4. Enable VPC Flow Logs

**Goal:** Configure VPC Flow Logs to capture all traffic and send logs to S3.

**Steps:**

1. Select the created VPC (`MyAuditVPC`)
2. Click **Actions** → **Create flow log**
3. Configure the flow log:

| Field               | Value                            |
|---------------------|----------------------------------|
| Filter              | All (accepted and rejected)      |
| Destination         | Send to an S3 bucket             |
| Destination S3 ARN  | `arn:aws:s3:::vpc-flow-logs-bucket` |
| IAM Role            | `VPCFlowLogsToS3Role`            |

4. Click **Create flow log**

---

## 📁 Files Included

| File                         | Description                                      |
|------------------------------|-------------------------------------------------|
| `s3/bucket-policy.json`       | S3 bucket policy allowing VPC Flow Logs writing |
| `iam/role-trust-policy.json`  | IAM role trust policy for EC2/VPC Flow Logs       |
| `iam/vpc-flow-logs-s3-policy.json` | IAM policy to allow PutObject on the S3 bucket   |

---
![Image](https://github.com/user-attachments/assets/8d1af4ca-a11f-4c94-bbca-ac4ea85bc143)
