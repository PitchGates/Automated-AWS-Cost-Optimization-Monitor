# 🤖 Automated AWS Cost Optimization Monitor

This project automates AWS cost monitoring and alerting to help you track spending, detect cost anomalies, and stay within your budget — all without manual effort.  
It leverages **AWS Lambda**, **CloudWatch**, and the **Budgets API** to analyze your account usage and send automated alerts via **Slack** and **SNS email notifications**.

---

## 🧠 Project Overview

Keeping track of AWS costs manually can be time-consuming and error-prone.  
This solution provides a **serverless, event-driven cost optimization monitor** that:
- Continuously checks account spending against predefined thresholds.  
- Sends **real-time cost alerts** to Slack or email via SNS.  
- Can be easily deployed using **AWS SAM** or **Terraform**.

It’s designed to **demonstrate AWS automation and cost management skills** suitable for real-world DevOps, FinOps, or cloud engineering portfolios.

---

## Architecture Overview

Architecture Diagram<img width="1536" height="1024" alt="No IAM Role_CloudWatch → Lambda → AWS Budgets API → Slack Webhook" src="https://github.com/user-attachments/assets/36bb5382-51bb-4a2e-a803-94edcf9d4b87" />
<img width="1536" height="1024" alt="CloudWatch → Lambda → AWS Budgets API → Slack Webhook" src="https://github.com/user-attachments/assets/95f6f2ab-0aba-48be-b6a3-753348e82388" />


### 🧩 Components
| Layer              | Service                                | Purpose                                                     |
|--------------------|----------------------------------------|-------------------------------------------------------------|
| **Compute**        | AWS Lambda (Python 3.x)                | Runs cost-check logic periodically.                         |
| **Orchestration**  | Amazon CloudWatch Events / EventBridge | Schedules Lambda execution daily or hourly.                 |
| **Monitoring**     | AWS Budgets API                        | Fetches and analyzes account spending data.                 |
| **Notifications**  | Slack Webhook / Amazon SNS             | Delivers alerts when budgets are exceeded.                  |
| **Security**       | AWS IAM, Secrets Manager               | Manages permissions and securely stores Slack webhook URLs. |
| **IaC**            | AWS SAM / Terraform                    | Enables quick and repeatable deployment.                    |

---

## ⚙️ Setup & Deployment

### Clone the repository
```bash
git clone https://github.com/PitchGates/Automated-AWS-Cost-Optimization-Monitor.git
cd Automated-AWS-Cost-Optimization-Monitor

## Configure environment variables
In your Lambda function settings or .env file:
SLACK_WEBHOOK_URL = "https://hooks.slack.com/services/XXXX/XXXX"
SNS_TOPIC_ARN = "arn:aws:sns:us-east-1:123456789012:CostAlerts"
BUDGET_NAME = "MonthlyBudget"

## Deploy using AWS SAM
sam build
sam deploy --guided

## Test the monitor
Manually invoke the Lambda or wait for the scheduled EventBridge trigger.
If costs exceed your defined threshold, you’ll receive:
- A Slack message (via webhook)
- An SNS email alert
- A detailed cost report in CloudWatch Logs

## Tech Stack
| Category                   | Tools                                  |
| -------------------------- | -------------------------------------- |
| **Compute**                | AWS Lambda (Python 3.x)                |
| **Orchestration**          | Amazon CloudWatch Events / EventBridge |
| **Monitoring**             | AWS Budgets API                        |
| **Notifications**          | Slack Incoming Webhook, Amazon SNS     |
| **Security**               | AWS IAM, AWS Secrets Manager           |
| **Infrastructure as Code** | AWS SAM / Terraform                    |

## Future Improvements
Integrate AWS Cost Explorer API for deeper insights.
Add anomaly detection using Amazon Lookout for Metrics.
Include daily budget trend visualizations.
Build a simple web dashboard for multi-account monitoring.
