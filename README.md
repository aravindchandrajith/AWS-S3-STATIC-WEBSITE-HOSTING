#  Personal Portfolio — Hosted on AWS S3 + CloudFront

A fully static personal portfolio website deployed on **AWS S3** with **CloudFront CDN**, custom domain via **Route 53**, HTTPS via **ACM**, and automated CI/CD using **GitHub Actions**.

![AWS S3](https://img.shields.io/badge/AWS-S3-orange?logo=amazon-aws)
![CloudFront](https://img.shields.io/badge/AWS-CloudFront-orange?logo=amazon-aws)
![GitHub Actions](https://img.shields.io/badge/CI/CD-GitHub_Actions-blue?logo=github-actions)

---

##  Architecture

```
Developer → GitHub Push
              ↓
        GitHub Actions (CI/CD)
              ↓
         AWS S3 Bucket
        (Static Hosting)
              ↓
       CloudFront CDN
     (Global Edge Network)
              ↓
     Route 53 (DNS) + ACM (HTTPS)
              ↓
          End User 
```

---

## 🚀 AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon S3** | Store and serve static files (HTML, CSS, JS) |
| **CloudFront** | CDN — caches content at edge locations globally |
| **Route 53** | DNS management, routes domain to CloudFront |
| **ACM (Certificate Manager)** | Free SSL/TLS certificate for HTTPS |
| **IAM** | Secure access keys for GitHub Actions deployment |

---

##  Setup & Deployment Steps

### 1. Created S3 Bucket
```bash
# Create bucket
aws s3 mb s3://aws-static-website-hosting --region us-east-1

# Enable static website hosting
aws s3 website s3://aws-static-website-hosting \
  --index-document index.html \
  --error-document index.html
```

### 2. Set-up Bucket Policy (Public Read)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::aws-static-website-hosting/*"
    }
  ]
}
```

### 3. Createdd CloudFront Distribution
- Origin: your S3 bucket website endpoint
- Default root object: `index.html`
- HTTPS: Redirect HTTP to HTTPS
- SSL certificate: Request from ACM (us-east-1 region)

### 4. Configure Route 53
- Created a hosted zone for the domain
- Added an **A record (Alias)** pointing to CloudFront distribution

### 5. GitHub Actions CI/CD
Added these secrets to the GitHub repo:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `S3_BUCKET_NAME`
- `CLOUDFRONT_DISTRIBUTION_ID`

Every push to `main` automatically deploys and invalidates CloudFront cache.

---

##  Key Concepts Demonstrated

- **Static Website Hosting on S3**: Serving HTML/CSS/JS files directly from S3 without any server
- **CDN with CloudFront**: Reduces latency by caching content at 400+ global edge locations
- **HTTPS Security**: ACM provides free SSL certificates, CloudFront enforces HTTPS
- **Infrastructure as Code mindset**: Reproducible deployment via GitHub Actions
- **Cost Efficiency**: S3 static hosting costs ~$0.023/GB — near zero for a portfolio site

---

##  Cost Breakdown (Approximate)

| Service | Monthly Cost |
|---|---|
| S3 Storage (< 1 GB) | ~$0.02 |
| CloudFront (< 10 GB transfer) | ~$0.85 |
| Route 53 Hosted Zone | $0.50 |
| ACM Certificate | **Free** |
| **Total** | **~$1.37/month** |

---

##  Connect

- LinkedIn: [linkedin.com/in/aravindchandrajith](https://linkedin.com)
- Email: aravindchandrajith@example.com

