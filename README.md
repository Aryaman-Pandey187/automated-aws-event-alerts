# 🎯 Automated Daily-Events Email Alert (AWS Lambda + EventBridge + S3)

This project reads an Excel file stored in an S3 bucket, checks if today's date matches any event listed, and sends a Gmail notification if a match is found. It runs automatically every day at **12:01 AM IST** using **AWS Lambda** and **Amazon EventBridge Scheduler**.

---

## ✅ Features

- Reads `events.xlsx` file from S3
- Matches today's date (`dd-mm`) with event rows
- Securely fetches Gmail credentials from AWS Secrets Manager
- Sends email via Gmail SMTP
- Fully automated with daily schedule (12:01 AM IST)
- Secure IAM role and permissions setup

---

## 📁 Project Structure

