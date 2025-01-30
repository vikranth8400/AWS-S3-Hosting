# AWS-S3-Hosting
# Hosting a Static Website on Amazon S3 with Custom Domain Integration
## Overview
This project demonstrates how to host a static website using Amazon S3, AWS’s scalable cloud storage service, and connect it to a custom domain using AWS Route 53. 
The goal is to create a publicly accessible website while learning core AWS tools and security best practices. Amazon Simple Storage Service (Amazon S3) is an object storage service that provides high availability, security, and scalability. This guide will walk you through hosting a static website on **Amazon S3**, making it publicly accessible, securing it with **ACLs and Bucket Policies**, enabling **Versioning**, and linking it with a **Custom Domain** via **AWS Route 53**.

---

## Prerequisites
Before starting, ensure you have:
- An **AWS account** with access to **S3** and **Route 53**.
- Basic knowledge of **HTML** and **AWS services**.
- An **index.html** file and related website assets ready for upload.

---

## Step 1: Create an S3 Bucket
### 1.1 Open Amazon S3
- Log in to the **AWS Management Console**.
- Search for **S3** in the search bar.
- Click on **S3** to access the dashboard.

### 1.2 Create a Bucket
- Click **Create bucket**.
- Set **Bucket Name** (e.g., `your-name-website-project`).
  - Bucket names must be **globally unique** and cannot contain **uppercase characters**.
- Under **Object Ownership**, select **ACLs enabled**.
- Under **Block Public Access settings**, uncheck **Block all public access**.
- Check the acknowledgment box.
- Enable **Bucket Versioning**.
- Click **Create bucket**.

---

## Step 2: Upload Website Content
### 2.1 Upload Files to S3
- Open the **S3 bucket**.
- Go to the **Objects** tab.
- Click **Upload**.
- Click **Add files** and select `index.html`.
- Click **Add folder** and select the folder containing website assets.
- Click **Upload**.

---

## Step 3: Configure Static Website Hosting
### 3.1 Enable Static Website Hosting
- Open your **S3 bucket**.
- Go to the **Properties** tab.
- Scroll to **Static website hosting**.
- Click **Edit**.
- Under **Static website hosting**, select **Enable**.
- Under **Hosting type**, choose **Host a static website**.
- Set **Index document** to `index.html`.
- Click **Save changes**.
- Note the **Bucket website endpoint** (your website URL).

### 3.2 Make Website Files Public
By default, S3 objects are private. To make them public:
- Go to the **Objects** tab in your **S3 bucket**.
- Select the `index.html` file and other assets.
- Click **Actions** > **Make public using ACL**.

---

## Step 4: Secure Your Bucket
### 4.1 Use Bucket Policies to Prevent Deletion
To protect critical files, add a **bucket policy**:
- Go to the **Permissions** tab of your S3 bucket.
- Scroll to **Bucket Policy**.
- Click **Edit** and enter:
  
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
        "Resource": "arn:aws:s3:::your-name-website-project/index.html"
      }
    ]
  }
  ```
- Click **Save changes**.

### 4.2 Pre-Signed URLs for Secure Access
For temporary secure access:
- Use **AWS CLI** or SDK to generate a **pre-signed URL**.
- The pre-signed URL expires after a set time, restricting access.

---

## Step 5: Enable Bucket Versioning
### 5.1 Turn On Versioning
- Go to **S3 bucket** > **Properties**.
- Scroll to **Bucket Versioning**.
- Click **Enable**.
- Once enabled, all object versions are stored.
- Deleted files can be restored if versioning is on.

---

## Step 6: Configure a Custom Domain (AWS Route 53)
### 6.1 Register a Domain
- Go to **AWS Management Console**.
- Search for **Route 53**.
- Click **Domain Registration** > **Register Domain**.
- Search for an available domain name.
- Complete the purchase.

### 6.2 Create a New S3 Bucket for Your Domain
- Create a **new S3 bucket**.
- **Bucket name must match the domain name** (e.g., `yourdomain.com`).
- Enable **Static website hosting**.
- Upload `index.html` and assets.

### 6.3 Link the Domain to S3
- Open **Route 53**.
- Go to **Hosted Zones** and select your domain.
- Click **Create Record**.
  - **Record type**: A (Routes traffic to an AWS resource).
  - **Value/Route traffic to**:
    - **Choose endpoint**: Alias to S3 website endpoint.
    - **Region**: The region where your S3 bucket is hosted.
    - **S3 bucket**: Select the bucket matching your domain name.
  - Disable **Evaluate target health**.
- Click **Create Record**.

---

## Conclusion
I have successfully hosted a **Static Website on Amazon S3**, secured it with **ACLs and Bucket Policies**, enabled **Versioning**, and linked it with a **Custom Domain using AWS Route 53**.

---

## Additional Resources
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [AWS Route 53 Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/)
- [AWS Security Best Practices](https://aws.amazon.com/security/best-practices/)

---

## Author
**Vikranth Pulahari**
