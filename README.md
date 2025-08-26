# Automated AWS Cost Optimization Monitor

## 📌 Project Status
🚧 **Active Development** - This project is currently in the planning and initial implementation phase. Last updated: 2025-08-26.

## 🎯 Objective
To build a serverless cost optimization tool for AWS that proactively monitors spending and alerts teams via Slack when budget thresholds are exceeded, preventing bill shocks and promoting financial accountability.

## 🏗️ Architecture & Workflow
1.  **Trigger:** An AWS CloudWatch Event Rule triggers a Lambda function on a daily schedule.
2.  **Logic:** The Lambda function (Python), using the AWS Budgets API, checks the current spend against defined thresholds.
3.  **Notification:** If a threshold is breached, the function sends a detailed alert to a designated Slack channel via a webhook.
4.  **Security:** IAM roles with least-privilege policies grant only the necessary permissions (e.g., `budgets:ViewBudget`, `secretsmanager:GetSecretValue`).

## 🔧 Tech Stack
-   **Compute:** AWS Lambda (Python 3.x)
-   **Orchestration:** Amazon CloudWatch Events / EventBridge
-   **Monitoring:** AWS Budgets API
-   **Notifications:** Slack Incoming Webhook
-   **Security:** AWS IAM, AWS Secrets Manager (for storing webhook URL)
-   **Infrastructure as Code:** AWS SAM / Terraform

## ✅ Next Steps
-   [ ] Create and configure a Slack Incoming Webhook.
-   [ ] Develop the Python Lambda function logic to fetch budget data.
-   [ ] Implement the code to format and post messages to Slack.
-   [ ] Define the IAM execution role with least-privilege permissions.
-   [ ] Create the CloudWatch Event rule for daily invocation.
-   [ ] Deploy the stack using AWS SAM or Terraform.

## 📂 Repository Structure
