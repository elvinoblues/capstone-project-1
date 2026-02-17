☁️ AWS Static Website Deployment (S3 & CloudFront)
📖 Project Overview
This capstone project demonstrates the deployment of a highly available, scalable, and secure static website using Amazon Web Services (AWS).

The solution leverages Amazon S3 for durable object storage and Amazon CloudFront as a Content Delivery Network (CDN) to cache content at edge locations globally, reducing latency and improving load times for end-users. Security is enforced by restricting direct S3 bucket access and ensuring all traffic flows through CloudFront via Origin Access Control (OAC).

🏗️ Architecture
Data Flow:

User Request: The client accesses the website via the CloudFront domain (or custom domain).

Edge Caching: CloudFront checks if the requested content is cached at the nearest Edge Location.

Hit: Content is served immediately.

Miss: CloudFront forwards the request to the S3 Origin.

Origin Fetch: S3 validates the request against the Bucket Policy (allowing only CloudFront OAC).

Response: S3 returns the content to CloudFront, which caches it and serves it to the user.

🛠️ Tech Stack
Cloud Provider: AWS

Storage: Amazon S3 (Simple Storage Service)

CDN: Amazon CloudFront

Security: AWS IAM (Bucket Policies), Origin Access Control (OAC)

DNS (Optional): Amazon Route 53

SSL/TLS (Optional): AWS Certificate Manager (ACM)

✅ Prerequisites
An active AWS Account.

AWS CLI installed and configured (optional, if deploying via CLI).

Basic static website files (index.html, style.css, images, etc.).

🚀 Deployment Guide
Phase 1: Storage Setup (S3)
Create an S3 Bucket:

Name: my-capstone-website-[unique-id]

Region: us-east-1 (or your preferred region)

Block Public Access: Enabled (True). Note: We are not making the bucket public directly.

Bucket Versioning: Enabled (Recommended for recovery).

Upload Content:

Upload index.html and other assets to the bucket root.

Phase 2: Content Delivery Network (CloudFront)
Create Distribution:

Origin Domain: Select the S3 bucket created in Phase 1.

Origin Access: Select "Origin access control settings (recommended)".

Create Control Setting: Create a new OAC setting (sign requests).

Viewer Protocol Policy: Redirect HTTP to HTTPS.

Default Root Object: index.html.

Deploy: Click "Create Distribution".

Phase 3: Security Configuration
Update S3 Bucket Policy:

CloudFront will provide a policy statement after creation. Copy this policy.

Go to S3 Console > Permissions > Bucket Policy.

Paste the policy to allow the CloudFront principal (Service: cloudfront.amazonaws.com) to perform s3:GetObject.

Sample Bucket Policy:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::st40demo-elvis/*"
        },
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::st40demo-elvis/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::520827483261:distribution/E23SAB6RPA5NEN"
                }
            }
        }
    ]
}

🧪 Validation & Testing
Wait for the CloudFront distribution status to change from Deploying to Enabled.

Copy the Distribution Domain Name.

Paste the URL into a browser.

you should see the static website.

🔮 Future Improvements
Custom Domain: Configure a custom domain (e.g., www.example.com) using Route 53.

CI/CD Pipeline: Implement GitHub Actions or AWS CodePipeline to automatically sync the S3 bucket when code is pushed to the repository.

Infrastructure as Code (IaC): Automate the entire infrastructure provisioning using Terraform or AWS CloudFormation.

👨‍💻 Author
[Ngozi Elvis Ujedibie]
[https://www.linkedin.com/in/elvis-ujedibie]
