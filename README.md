# Static Website on S3 + CloudFront (with OAC)

## 📌 Overview
This project demonstrates hosting a static website using **Amazon S3** and serving it securely through **Amazon CloudFront**.  
The setup uses **Origin Access Control (OAC)** to keep the S3 bucket private and enforce **HTTPS-only traffic**.

---

## 🏗 Architecture
![Architecture Diagram](./docs/diagram.png)

**Flow:**
1. User connects via CloudFront.  
2. CloudFront enforces HTTPS (redirects HTTP → HTTPS).  
3. CloudFront uses OAC to fetch content from a private S3 bucket.  
4. S3 hosts static files (HTML, CSS, JS) with **Block Public Access** enabled.  

---

## 🚀 Implementation

### 1. S3 Bucket
- Bucket created with **Block Public Access enabled**.  
- Static files uploaded:  
  - `index.html`  
  - `style.css`  
  - `script.js`  

![S3 Bucket Objects](./docs/bucket.png)

---

### 2. Bucket Policy
Bucket is private. Access is only allowed to CloudFront distribution via OAC.

![Bucket Policy](./docs/bucket_policy.png)

---

### 3. CloudFront Distribution
#### Origin
- Connected to S3 bucket (REST endpoint) with OAC.  

![CloudFront Origin](./docs/cfn_origin.png)

#### Behavior
- Redirect HTTP → HTTPS.  
- Optimized caching (recommended for S3).  

![CloudFront Behavior](./docs/cfn_behavior.png)

#### General Settings
- Default root object = `index.html`.  

![CloudFront General](./docs/cfn_general.png)

---

### 4. Website Access
The website is available securely via CloudFront domain:  
👉 `https://dcbg5pac4c8ja.cloudfront.net`

![Website Demo](./docs/website-test.png)

---

## 🔐 Security & Best Practices
- ✅ **OAC** used → S3 bucket is fully private.  
- ✅ **Block Public Access** enabled for S3.  
- ✅ **Redirect HTTP → HTTPS** for secure in-transit communication.  
- ✅ **Default root object** ensures predictable entry point.  
- 💡 In production, custom domain + ACM TLS cert could be added.  

---

## 📚 Lessons Learned
- Difference between **S3 website endpoint** vs **REST endpoint**.  
- How to configure **OAC** instead of legacy OAI.  
- How CloudFront enforces **HTTPS-only access**.  
