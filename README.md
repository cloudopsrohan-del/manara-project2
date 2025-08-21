PROJECT2
# Project 2: Serverless Image Processing with AWS S3 and Lambda

## 📌 Description
This project implements a **serverless image processing pipeline** on AWS. When a user uploads an image to an S3 bucket, a Lambda function is triggered to resize (and optionally watermark) the image, then store the processed image in a separate S3 bucket.

---

## 🏗️ Architecture Diagram
![Architecture Diagram](./architecture-diagram.png)

---

## 🧱 AWS Services Used
- **Amazon S3** – stores original and processed images.
- **AWS Lambda** – processes images upon upload (resize, watermark).
- **Amazon API Gateway** *(optional)* – provides presigned upload URLs.
- **Amazon DynamoDB** *(optional)* – stores image metadata.
- **IAM Roles & Policies** – secure access control.

---

## ⚙️ How It Works
1. User uploads image to `raw-images-bucket`.
2. S3 triggers Lambda via event notification.
3. Lambda resizes & watermarks the image.
4. Lambda uploads the processed image to `processed-images-bucket`.
5. (Optional) Lambda writes metadata to DynamoDB.
6. (Optional) API Gateway provides presigned URL to upload without exposing AWS credentials.

---

## 📦 Deployment Steps
1. Create two S3 buckets:
   - `raw-images-bucket`
   - `processed-images-bucket`
2. Create a Lambda function:
   - Runtime: Python 3.12
   - Attach IAM policy to allow `GetObject` from raw bucket and `PutObject` to processed bucket.
   - Add environment variable `OUTPUT_BUCKET=processed-images-bucket`
3. Add S3 event trigger to Lambda for `raw-images-bucket` (`s3:ObjectCreated:*`).
4. (Optional) Set up DynamoDB table and add permissions.
5. (Optional) Add API Gateway with a Lambda to generate presigned URLs.

---

## 🧪 Testing
- Upload an image to the raw bucket:
  ```bash
  aws s3 cp test.jpg s3://raw-images-bucket/
