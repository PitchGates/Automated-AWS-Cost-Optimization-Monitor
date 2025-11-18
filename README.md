# Automated AWS Cost Optimization Monitor
A serverless solution built with Python and AWS Lambda to proactively identify and report cost-saving opportunities in your AWS environment. This tool automates the discovery of idle EC2 instances, underutilized EBS volumes, and obsolete EBS snapshots, helping you enforce FinOps principles and reduce unnecessary cloud spend.

🚀 Features
Idle EC2 Detection: Identifies EC2 instances with low CPU utilization over a defined period.

EBS Volume Check: Finds unattached or low-usage Elastic Block Store volumes.

Snapshot Cleanup: Locates old EBS snapshots that are no longer associated with active volumes.

Automated Reporting: Sends detailed findings directly to your email or Slack via Amazon SNS.

Serverless & Cost-Effective: Built entirely on AWS Lambda, ensuring minimal operational overhead and cost.

📋 Prerequisites
Before deploying this solution, ensure you have the following:

An AWS Account with appropriate permissions

AWS CLI configured with credentials that have permissions to create the necessary resources

Python 3.9+ (for local development and testing)

🛠️ Installation & Deployment
1. Clone the Repository
bash
git clone https://github.com/PitchGates/Automated-AWS-Cost-Optimization-Monitor.git
cd Automated-AWS-Cost-Optimization-Monitor
2. Deploy with AWS SAM (Serverless Application Model)
The easiest way to deploy this application is using the AWS SAM CLI.

bash
# Build the application
sam build

# Deploy the application
sam deploy --guided
During the guided deployment, you will be prompted to enter:

Stack Name: A name for your CloudFormation stack

AWS Region: Your preferred AWS Region

NotificationEmail: The email address where cost reports will be sent

Confirm changeset: Yes

Allow SAM CLI IAM role creation: Yes

3. Confirm SNS Subscription
After deployment, check your email and confirm the SNS subscription to start receiving reports.

⚙️ Configuration
Environment Variables
The Lambda function uses the following configurable parameters:

CPU_UTILIZATION_THRESHOLD: Minimum CPU utilization percentage (default: 5%)

VOLUME_UTILIZATION_THRESHOLD: Minimum volume read/write operations (default: 1 I/O)

SNAPSHOT_AGE_THRESHOLD: Maximum age for snapshots in days (default: 90)

EventBridge Schedule
The monitor runs daily at 9:00 AM UTC by default. To modify this schedule, edit the Events rule in the template.yaml file:

yaml
Events:
  CostMonitorSchedule:
    Type: Schedule
    Properties:
      Schedule: cron(0 9 * * ? *)  # Edit this expression
🏗️ Architecture
Found in the Architecture folder

This solution uses the following AWS services:
- AWS Lambda: Executes the cost optimization checks
- Amazon EventBridge: Triggers the function on a scheduled basis
- Amazon SNS: Delivers notification reports
- AWS IAM: Provides secure permissions for the Lambda function
- Amazon CloudWatch: Stores logs and metrics

📊 Sample Output
You will receive reports formatted like this:

text
AWS COST OPTIMIZATION REPORT - 2024-01-15

EC2 INSTANCES:
❌ us-east-1: i-1234567890abcdef (t3.medium) - Avg CPU: 2%
❌ us-west-2: i-0987654321abcdef (m5.large) - Avg CPU: 1%

EBS VOLUMES:
❌ vol-aaaaaaa - Unattached (50 GiB)
❌ vol-bbbbbbb - Low usage (100 GiB, <1 IOPS)

EBS SNAPSHOTS:
❌ snap-1111111 - 120 days old (50 GiB)
❌ snap-2222222 - 95 days old (100 GiB)

ESTIMATED MONTHLY SAVINGS: ~$185
🧪 Testing
Run Locally
bash
# Install dependencies
pip install -r requirements.txt

# Test the function locally
python -m pytest tests/
Manual Invocation
You can manually trigger the function via the AWS Console or CLI:

bash
aws lambda invoke --function-name AutomatedCostMonitorFunction response.json
📝 License
This project is licensed under the MIT License - see the LICENSE file for details.

🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

📞 Support
If you have any questions or run into issues, please open an issue on GitHub.

👨‍💻 Author
PitchGates

GitHub: @PitchGates

Acknowledgments
Inspired by AWS Well-Architected Framework's cost optimization pillar
Built using the AWS Serverless Application Model (SAM)
