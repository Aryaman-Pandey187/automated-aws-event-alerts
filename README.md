# 🎯 Automated Daily-Events Email Alert (AWS Lambda + EventBridge + S3 + Secrets Manager)

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

```

.
├── lambda\_function.py       # Main Lambda handler script
├── requirements.txt         # Python dependencies (pandas, openpyxl, boto3)
├── README.md                # Project documentation
└── /dependencies            # (Optional) Directory for zipped Python packages

````

---

## 🔐 AWS Services Used

| Service             | Purpose                                  |
|---------------------|-------------------------------------------|
| **Lambda**          | Run the Python script                    |
| **S3**              | Store the `events.xlsx` file             |
| **Secrets Manager** | Securely store Gmail credentials         |
| **EventBridge**     | Schedule daily Lambda execution          |
| **CloudWatch Logs** | Monitor logs and errors                  |

---

## 📦 Deploying the Lambda Function

### 1. Prepare Your ZIP

Include your `lambda_function.py` and dependencies (pandas, openpyxl, etc.) in a ZIP file:
```bash
zip -r9 lambda_package.zip .
````

> 💡 Tip: Use Docker or `pip install -t` inside a virtual environment with Python version matching Lambda runtime.

### 2. Upload to Lambda

* Runtime: `Python 3.12` (or your preferred supported version)
* Timeout: `15 seconds`
* Memory: `128 MB` or more if needed

---

## 🔑 Secrets Manager Setup

Create a secret in **AWS Secrets Manager**:

* Name: `gmail-smtp-credentials`
* Secret Value (as JSON):

```json
{
  "email": "your-email@gmail.com",
  "password": "your-app-password"
}
```

> 🔒 Use **App Passwords**, not your real Gmail password. App Passwords require enabling 2FA on your Gmail account.

---

## 🪣 S3 Bucket Setup

* Upload `events.xlsx` to your S3 bucket (e.g., `my-bucket`)
* File should have at least two columns:

  ```
  Date        | Event_Name
  ------------|---------------------
  10-01-2025  | John's Birthday
  15-07-2025  | Project Launch
  ```

---

## ⏰ EventBridge Scheduler Setup

Create a daily rule:

* Type: **Cron expression**
* Value: `cron(31 18 * * ? *)` → Runs **12:01 AM IST**
* Target: Your Lambda function
* Flexible Time Window: **Off**

---

## 🔐 IAM Role Permissions

Attach this inline policy to your Lambda execution role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:secretsmanager:ap-south-1:123456789012:secret:gmail-smtp-credentials-*",
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    }
  ]
}
```

Replace the region/account ID and bucket name as needed.

---

## 🧪 Testing

You can manually test the Lambda:

1. Go to Lambda Console
2. Click **Test**
3. Use `{}` as sample input
4. Check **CloudWatch Logs** for results

---

## 🛡️ Monitoring (Optional)

* Enable **CloudWatch Logs**
* Create CloudWatch Alarm for `Errors >= 1` daily
* Send notifications via **SNS email topic**

---

## 🧑‍💻 Author

**Aryaman Pandey**
📍 Chennai, India
🔗 [linkedin.com/in/aryamanpandey](https://www.linkedin.com/in/aryaman-pandey/)

---

## 📝 License

This project is released under the MIT License.

```
