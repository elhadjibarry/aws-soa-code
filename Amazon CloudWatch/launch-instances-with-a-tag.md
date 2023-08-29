# Launch instances with a tag
aws ec2 run-instances --image-id ami-0f409bae3775dc8e5 --count 3 --instance-type t2.micro --tag-specifications 'ResourceType=instance,Tags=[{Key=Department,Value=Operations}]'

# CloudWatch Logs Insights query
fields @timestamp, @message | sort @timestamp desc | limit 25
