import boto3
import json
import os
import urllib3

http = urllib3.PoolManager()
sns = boto3.client('sns')
budgets = boto3.client('budgets')

def lambda_handler(event, context):
    account_id = boto3.client('sts').get_caller_identity()['Account']
    budget_name = os.environ['BUDGET_NAME']
    sns_topic = os.environ['SNS_TOPIC_ARN']
    
    response = budgets.describe_budget(
        AccountId=account_id,
        BudgetName=budget_name
    )
    
    actual_spend = float(response['Budget']['CalculatedSpend']['ActualSpend']['Amount'])
    limit = float(response['Budget']['BudgetLimit']['Amount'])
    percent_used = (actual_spend / limit) * 100
    
    if percent_used > 80:
        message = f"AWS cost alert: {percent_used:.2f}% of budget used (${actual_spend:.2f}/${limit:.2f})."
        sns.publish(TopicArn=sns_topic, Message=message)
        
        # Slack integration (optional)
        try:
            secret_arn = os.environ['SLACK_SECRET_ARN']
            secrets_client = boto3.client('secretsmanager')
            secret_value = secrets_client.get_secret_value(SecretId=secret_arn)
            slack_url = json.loads(secret_value['SecretString'])['SLACK_WEBHOOK_URL']
            
            http.request('POST', slack_url, body=json.dumps({"text": message}), headers={'Content-Type': 'application/json'})
        except Exception as e:
            print(f"Slack notification failed: {e}")
    
    return {"statusCode": 200, "body": json.dumps("Budget check complete")}
