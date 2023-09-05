# Create private subnets and NAT gateway

## Create a private subnet in us-east-1a
aws ec2 create-subnet --vpc-id vpc-0459eb335386529fc --cidr-block 172.31.96.0/20 --availability-zone us-east-1a --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=default-private-1a}]'
## Create a private subnet in us-east-1b
aws ec2 create-subnet --vpc-id vpc-0459eb335386529fc --cidr-block 172.31.112.0/20 --availability-zone us-east-1b --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=default-private-1b}]'
## Create a route table in the default VPC
aws ec2 create-route-table --vpc-id vpc-0459eb335386529fc --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=Default-PrivateRT}]'
## Associate private subnets to the route table
aws ec2 associate-route-table --route-table-id rtb-0fbaf18bd60c64ed7 --subnet-id subnet-0ca5beafaae83cdeb
aws ec2 associate-route-table --route-table-id rtb-0fbaf18bd60c64ed7 --subnet-id subnet-0143250d6b244a643
## Create an Elastic IP
aws ec2 allocate-address
## Create a NAT gateway
aws ec2 create-nat-gateway --subnet-id subnet-0ee064afaa8d4292a --allocation-id eipalloc-0c2f1316a6b4c857c
## Update the private route table to point to the NAT gateway
aws ec2 create-route --route-table-id rtb-0fbaf18bd60c64ed7 --destination-cidr-block 0.0.0.0/0 --nat-gateway-id nat-0910a959a27e71072

# Launch EC2 instance

1. Create a security group
aws ec2 create-security-group --group-name NAT-GW-LAB --description "Temporary SG for the NAT gateway Lab"
2. Launch instance in US-EAST-1A
aws ec2 run-instances --image-id ami-005f9685cb30f234b --instance-type t2.micro --subnet-id subnet-0ca5beafaae83cdeb --security-group-ids sg-0d12d6533dc951063 --iam-instance-profile Name=SSMInstanceProfile