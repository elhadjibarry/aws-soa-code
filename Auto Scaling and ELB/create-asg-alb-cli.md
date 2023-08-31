
## Create a security group

aws ec2 create-security-group --group-name ALB-EC2-Access --description "Route 53 Policy Test" --region us-east-1

aws ec2 authorize-security-group-ingress --group-name ALB-EC2-Access --protocol tcp --port 22 --cidr 0.0.0.0/0 --region us-east-1

aws ec2 authorize-security-group-ingress --group-name ALB-EC2-Access --protocol tcp --port 80 --cidr 0.0.0.0/0 --region us-east-1

## create auto scaling group

aws autoscaling create-auto-scaling-group --auto-scaling-group-name ASG1 --launch-template "LaunchTemplateName=LT1" --min-size 1 --max-size 3 --desired-capacity 2 --availability-zones "us-east-1a" "us-east-1b" --vpc-zone-identifier "subnet-0ee064afaa8d4292a, subnet-03e42d391edc17648"

## create target group, load balancer, listener, and then link it all up

aws elbv2 create-target-group --name TG1 --protocol HTTP --port 80 --vpc-id vpc-0459eb335386529fc

aws elbv2 create-load-balancer --name ALB1 --subnets subnet-0ee064afaa8d4292a subnet-03e42d391edc17648 --security-groups sg-082499976ff78c4eb

aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:291934546285:loadbalancer/app/ALB1/b422a061b4a3382f --protocol HTTP --port 80 --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:291934546285:targetgroup/TG1/a1f2be0798f0b11c

aws autoscaling attach-load-balancer-target-groups --auto-scaling-group-name ASG1 --target-group-arns arn:aws:elasticloadbalancing:us-east-1:291934546285:targetgroup/TG1/a1f2be0798f0b11c

## delete ASG and ALB

aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:291934546285:loadbalancer/app/ALB1/b422a061b4a3382f

aws autoscaling delete-auto-scaling-group --auto-scaling-group-name ASG1 --force-delete

aws elbv2 delete-target-group --target-group-arn arn:aws:elasticloadbalancing:us-east-1:291934546285:targetgroup/TG1/a1f2be0798f0b11c