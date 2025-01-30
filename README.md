# AWS-S3-Hosting
# Hosting a Static Website on Amazon S3 with Custom Domain Integration
## Overview
This project demonstrates how to host a static website using Amazon S3, AWS’s scalable cloud storage service, and connect it to a custom domain using AWS Route 53. 
The goal is to create a publicly accessible website while learning core AWS tools and security best practices. Amazon Simple Storage Service (Amazon S3) is an object storage service that provides high availability, security, and scalability. This guide will walk you through hosting a static website on **Amazon S3**, making it publicly accessible, securing it with **ACLs and Bucket Policies**, enabling **Versioning**, and linking it with a **Custom Domain** via **AWS Route 53**.

---

## Understanding Key Concepts

### 1. Amazon S3 (Simple Storage Service)
Amazon S3 is an **object storage service** designed to store and retrieve any amount of data at any time. It provides high **durability, scalability, and security**. Unlike traditional file systems, S3 stores data in **buckets** and follows an **eventual consistency model**.

### 2. Static Website Hosting
A **static website** consists of **HTML, CSS, and JavaScript** files that do not require backend processing. Hosting on S3 allows users to access these files via a **bucket website endpoint**.

### 3. Access Control Lists (ACLs) and Bucket Policies
**ACLs** and **Bucket Policies** define permissions for objects in an S3 bucket:
- **ACLs** control access to individual objects.
- **Bucket Policies** apply permissions at the bucket level, making access control simpler.

### 4. Pre-Signed URLs
A **pre-signed URL** allows secure access to specific S3 objects for a limited time. This is useful for **temporary sharing of restricted content**.

### 5. Versioning
**Bucket Versioning** enables multiple versions of an object to be stored, allowing recovery from accidental overwrites or deletions.

### 6. Route 53 and Domain Hosting
Amazon **Route 53** is a scalable **Domain Name System (DNS)** service that helps in domain registration and routing traffic to AWS resources like S3 buckets.

---

## Step 1: Creating an S3 Bucket
### 1.1 Open Amazon S3
- Logged in to the **AWS Management Console**.
- Searched for **S3** in the search bar.
- Clicked on **S3** to access the dashboard.
! https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/2.png
### 1.2 Created a Bucket
- Clicked **Create bucket**.
- Set **Bucket Name** (e.g., `my-website-project`).
- Bucket names must be **globally unique** and cannot contain **uppercase characters**.
- Selected **ACLs enabled** for **Object Ownership**.
- Disabled **Block all public access** and acknowledged the settings.
- Enabled **Bucket Versioning**.
- Clicked **Create bucket**.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/4.png
---

## Step 2: Uploading Website Content
### 2.1 Uploaded Files to S3
- Opened the **S3 bucket**.
- Went to the **Objects** tab.
- Clicked **Upload**.
- Added `index.html` and related assets.
- Clicked **Upload**.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/5.png
---

## Step 3: Configuring Static Website Hosting
### 3.1 Enabled Static Website Hosting
- Opened the **S3 bucket**.
- Navigated to the **Properties** tab.
- Scrolled to **Static website hosting**.
- Enabled **Host a static website**.
- Set **Index document** to `index.html`.
- Saved changes and noted the **Bucket website endpoint**.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/7.png

### 3.2 Made Website Files Public
By default, S3 objects are private. To make them public:
- Went to the **Objects** tab in the S3 bucket.
- Selected `index.html` and other assets.
- Clicked **Actions** > **Make public using ACL**.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/9.png
---

## Step 4: Securing the Bucket
### 4.1 Used Bucket Policies to Prevent Deletion
Added a **bucket policy** to prevent accidental deletion:
- Opened the **Permissions** tab in S3.
- Edited the **Bucket Policy** and added:

  ```json
  {
    "Version": "2012-10-17",
    "Id": "MyBucketPolicy",
    "Statement": [
      {
        "Sid": "BucketPutDelete",
        "Effect": "Deny",
        "Principal": "*",
        "Action": "s3:DeleteObject",
        "Resource": "arn:aws:s3:::my-website-project/index.html"
      }
    ]
  }
  ```
- Saved changes.

### 4.2 Used Pre-Signed URLs for Secure Access
For temporary secure access:
- Used **AWS CLI** or SDK to generate a **pre-signed URL**.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/12.png
- The pre-signed URL expired after a set time, restricting access.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/13.png
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/16.png
---

## Step 5: Enabling Bucket Versioning
### 5.1 Turned On Versioning
- Opened **S3 bucket** > **Properties**.
- Enabled **Bucket Versioning**.
- Verified that all object versions were stored.
 https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/6.png 
---

## Step 6: Configuring a Custom Domain with Route 53
### 6.1 Registered a Domain
- Opened **Route 53**.
- Went to **Domain Registration** > **Registered Domain**.
- Chose an available domain name and completed the purchase.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/21.png

### 6.2 Created a New S3 Bucket for the Domain
- Created a **new S3 bucket** matching the domain name (e.g., `mydomain.com`).
- Enabled **Static website hosting**.
- Uploaded `index.html` and assets.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/23.png
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/24.png


### 6.3 Linked the Domain to S3
- Opened **Route 53** > **Hosted Zones**.
- Created an **A Record** to route traffic to the S3 bucket.
  - Selected **Alias to S3 website endpoint**.
  - Chose the appropriate **AWS Region**.
  - Disabled **Evaluate target health**.
- Saved changes.
https://github.com/vikranth8400/AWS-S3-Hosting/blob/544c4be3a4dfdbb778ecdb468f630f4f27675b49/25.png
https://raw.githubusercontent.com/vikranth8400/AWS-S3-Hosting/544c4be3a4dfdbb778ecdb468f630f4f27675b49/26.png

---

## Conclusion
Hosting a website using AWS S3 and Route 53 offers a scalable, cost-effective, and highly available solution for static web hosting. By leveraging Amazon S3’s durable storage capabilities and Route 53’s robust DNS management, we can ensure a reliable and secure web presence with minimal operational overhead. Additionally, integrating CloudFront for content delivery and AWS Certificate Manager for SSL/TLS encryption enhances performance and security, providing an optimized user experience.  

This approach is ideal for static websites, marketing pages, and documentation platforms, where low latency and high availability are essential. By utilizing AWS services, businesses can benefit from reduced infrastructure costs while maintaining flexibility for future scalability and integrations. As cloud computing continues to evolve, leveraging AWS-managed services ensures resilience, efficiency, and long-term sustainability in website hosting.  

This setup provides a **Cost-Effective, Scalable, and Reliable** method for hosting static websites. 

---

## Additional Resources
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [AWS Route 53 Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/)
- [AWS Security Best Practices](https://aws.amazon.com/security/best-practices/)

---

## Author
**Vikranth**
