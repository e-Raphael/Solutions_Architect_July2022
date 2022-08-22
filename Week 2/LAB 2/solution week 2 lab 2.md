
aws ec2 create-vpc --cidr-block 120.20.0.0/16
aws ec2 create-subnet --vpc-id vpc-06d24cca025869bf9 --cidr-block 120.20.1.0/24
aws ec2 create-subnet --vpc-id vpc-06d24cca025869bf9 --cidr-block 120.20.0.1/24
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id igw-0b936450f3516f965 --vpc-id vpc-06d24cca025869bf9
aws ec2 create-route-table --vpc-id vpc-06d24cca025869bf9
aws ec2 create-route --route-table-id rtb-07a7ca7744dbe1b55 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-0b936450f3516f965
aws ec2 associate-route-table --subnet-id subnet-0c4f36a4bdd4ee372 --route-table-id rtb-07a7ca7744dbe1b55
aws ec2 modify-subnet-attribute --subnet-id subnet-0c4f36a4bdd4ee372 --map-public-ip-on-launch

#launching an instance 
aws ec2 create-key-pair --key-name solution-architect-track --query 'KeyMaterial' --output text > solution-architect-track.pem
chmod 400 solution-architect-track.pem
aws ec2 create-security-group --group-name solution-architect-week2-lab --description "security group for week 2 lab" --vpc-id vpc-06d24cca025869bf9
aws ec2 authorize-security-group-ingress --group-id sg-01fbcdb3c54bf16c4 --protocol tcp --port 22 --cidr 0.0.0.0/0

aws ec2 run-instances --image-id ami-5c98fe23 --instance-type t2.micro --key-name solution-architect-track --security-group-ids sg-01fbcdb3c54bf16c4 --subnet-id subnet-0c4f36a4bdd4ee372
aws ec2 describe-instances --instance-id i-02be31799ffd42c8e
ssh -i "solution-architect-track.pem" ec2-user@54.227.200.234
sudo yum update-y

#clean up
aws ec2 terminate-instances --instance-id i-02be31799ffd42c8e
aws ec2 describe-instances --instance-id i-02be31799ffd42c8e
aws ec2 delete-security-group --group-id sg-01fbcdb3c54bf16c4
aws ec2 delete-subnet --subnet-id subnet-0734117549583d377
aws ec2 delete-subnet --subnet-id subnet-0c4f36a4bdd4ee372
aws ec2 delete-route-table --route-table-id rtb-07a7ca7744dbe1b55
aws ec2 detach-internet-gateway --internet-gateway-id igw-0b936450f3516f965
aws ec2 detach-internet-gateway --internet-gateway-id igw-0b936450f3516f965 --vpc-id vpc-06d24cca025869bf9
aws ec2 delete-internet-gateway --internet-gateway-id igw-0b936450f3516f965
aws ec2 delete-vpc --vpc-id vpc-06d24cca025869bf9
