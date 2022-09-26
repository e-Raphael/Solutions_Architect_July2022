#creation of vpc, 2 subnets in 2 AZs, Security group with ports 80 and 443, and 2 instances launch
aws ec2 create-vpc --cidr-block  10.20.0.0/16
aws ec2 create-subnet --vpc-id vpc-0da10c43964ebd296 --cidr-block 10.20.1.0/24 --availability-zone us-east-1a
aws ec2 create-subnet --vpc-id vpc-0da10c43964ebd296 --cidr-block 10.20.2.0/24 --availability-zone us-east-1b
aws ec2 create-security-group --group-name ALB-SG --description "security group for ALB" --vpc-id vpc-0da10c43964ebd296
aws ec2 authorize-security-group-ingress --group-id sg-07a0037f4c76b9852 --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-07a0037f4c76b9852 --protocol tcp --port 443 --cidr 0.0.0.0/0
aws ec2 run-instances --image-id ami-024fc608af8f886bc --instance-type t2.micro --subnet-id subnet-033f6abd61a20089a --security-group-ids sg-07a0037f4c76b9852 --associate-public-ip-address --key-name schull-key
aws ec2 run-instances --image-id ami-024fc608af8f886bc --instance-type t2.micro --subnet-id subnet-012dcb31a951431ef --security-group-ids sg-07a0037f4c76b9852 --associate-public-ip-address --key-name schull-key

#CREATION OF APPLICATION LOAD BALANCER
aws ec2 create-security-group --group-name APP_LOAD_BAL-SG --vpc-id vpc-0da10c43964ebd296 --description "security group for application load balancer"
aws ec2 authorize-security-group-ingress --group-id sg-0ac1d61da1ca40ae7 --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id igw-058a4b9e65ba3433c --vpc-id vpc-0da10c43964ebd296

aws elbv2 create-load-balancer --name schull-load-balancer --subnets subnet-033f6abd61a20089a subnet-012dcb31a951431ef --security-groups sg-0ac1d61da1ca40ae7

aws elbv2 create-target-group --name schull-target-group --protocol HTTP --port 80 --vpc-id vpc-0da10c43964ebd296 --ip-address-type ipv4

aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target-group/b68079195f6290f5 --targets Id=i-013d1d0a78560540c Id=i-06027b0224bc6869a

aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/app/schull-load-balancer/f786233f139d188e --protocol HTTP --port 80 --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target-group/b68079195f6290f5

aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/app/schull-load-balancer/f786233f139d188e

aws elbv2 delete-target-group --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target-group/b68079195f6290f5

aws ec2 stop-instances --instance-id i-013d1d0a78560540c
aws ec2 stop-instances --instance-id i-06027b0224bc6869a

#CREATION OF NETWORK LOAD BALANCER
aws elbv2 create-load-balancer --name schull-load-balancer --type network --subnets subnet-033f6abd61a20089a
aws elbv2 create-load-balancer --name schull-load-balancer-2 --type network --subnets subnet-012dcb31a951431ef 
aws elbv2 create-target-group --name schull-target --protocol TCP --port 80 --vpc-id vpc-0da10c43964ebd296
aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/b209c8f47dbe20ce --targets Id=i-013d1d0a78560540c Id=i-06027b0224bc6869a
aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/net/schull-load-balancer-2/31732713c92bfa2f --protocol TCP --port 80 --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/b209c8f47dbe20ce
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/net/schull-load-balancer-2/31732713c92bfa2f
aws elbv2 delete-target-group --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/b209c8f47dbe20ce

#CREATION OF GATEWAY LOAD BALANCER
aws elbv2 create-load-balancer --name schull-gateway-load-balancer --type gateway --subnets subnet-033f6abd61a20089a
aws elbv2 create-target-group --name schull-target --protocol GENEVE --port 6081 --vpc-id vpc-0da10c43964ebd296
aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/00a2697eaec937468a --targets Id=i-013d1d0a78560540c Id=i-06027b0224bc6869a
aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/gwy/schull-gateway-load-balancer/f5d2109cf91e3dbe --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/00a2697eaec937468a
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:loadbalancer/gwy/schull-gateway-load-balancer/f5d2109cf91e3dbe
aws elbv2 delete-target-group --target-group-arn arn:aws:elasticloadbalancing:us-east-1:326902989337:targetgroup/schull-target/00a2697eaec937468a
