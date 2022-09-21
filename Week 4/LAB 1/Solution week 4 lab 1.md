aws ec2 describe-vpcs
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-0531f9c462f4ea71f"
aws ec2 create-security-group --group-name schull-demo-SG --vpc-id vpc-0531f9c462f4ea71f --description "Security Group for schull hands-on"
aws ec2 create-key-pair --key-name schull-key --query 'KeyMaterial' --output text >schull-key.pem
chmod 400 schull-key.pem
aws ec2 run-instances --image-id ami-024fc608af8f886bc --instance-type t2.micro --subnet-id subnet-0b9cba12e6bd3937d --security-group-ids sg-07174d20e2381f176 --key-name schull-key --disable-api-termination  --count 1
aws ec2 describe-instances --instance-id i-03c2055f0ebf8cb5c
aws ec2 monitor-instances --instance-id i-03c2055f0ebf8cb5c
aws ec2 authorize-security-group-ingress --group-id sg-07174d20e2381f176 --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 modify-instance-attribute --instance-id i-03c2055f0ebf8cb5c --no-disable-api-termination
aws ec2 modify-instance-attribute --instance-id i-03c2055f0ebf8cb5c --no-disable-api-termination
aws ec2 terminate-instances --instance-id i-03c2055f0ebf8cb5c