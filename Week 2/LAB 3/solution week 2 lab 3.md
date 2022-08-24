aws ec2 create-vpc --cidr-block 100.20.0.0/16 --amazon-provided-ipv6-cidr-block
aws ec2 describe-vpcs --vpc-id vpc-09db7380650d26c
aws ec2 create-subnet --vpc-id vpc-09db7380650d26cdb --cidr-block 100.20.1.0/24 --ipv6-cidr-block 2600:1f18:6748:1400::/64
aws ec2 create-subnet --vpc-id vpc-09db7380650d26cdb --cidr-block 100.20.5.0/24 --ipv6-cidr-block 2600:1f18:6748:1401::/64
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-09db7380650d26cdb --internet-gateway-id igw-02d72b194d4496f20
aws ec2 create-route --route-table-id rtb-080f4e578dc2edb43 --destination-ipv6-cidr-block ::/0 --gateway-id igw-02d72b194d4496f20
aws ec2 create-route --route-table-id rtb-080f4e578dc2edb43 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-02d72b194d4496f20
aws ec2 associate-route-table --subnet-id subnet-0d05494854a75b87a --route-table-id rtb-080f4e578dc2edb43
aws ec2 create-egress-only-internet-gateway --vpc-id vpc-09db7380650d26cdb
aws ec2 create-route-table --vpc-id vpc-09db7380650d26cdb
aws ec2 create-route --route-table-id rtb-085942b2d344ec0ce --destination-ipv6-cidr-block ::/0 --egress-only-internet-gateway-id eigw-05ba28e8b004f65fe
aws ec2 modify-subnet-attribute --subnet-id subnet-0b07a5a5c6e0125ad --assign-ipv6-address-on-creation
aws ec2 modify-subnet-attribute --subnet-id subnet-0d05494854a75b87a --assign-ipv6-address-on-creation

aws ec2 create-key-pair --key-name week2-lab3-key --query "KeyMaterial" --output text > week2-lab3-key.pem
 chmod 400 week2-lab3-key.pem
aws ec2 create-security-group --group-name week2-lab3-SG --description "Security Group fro week 2 lab 3" --vpc-id vpc-09db7380650d26cdb
aws ec2 authorize-security-group-ingress --group-id sg-0bd13ca6c5e577e74 --ip-permissions '[{"IpProtocol": "tcp", "FromPort": 22, "ToPort": 22, "Ipv6Ranges": [{"CidrIpv6": "::/0"}]}]'
aws ec2 run-instances --image-id ami-0de53d8956e8dcf80 --count 1 --instance-type t2.micro --key-name week2-lab3-key --security-group-ids sg-0bd13ca6c5e577e74 --subnet-id subnet-0d05494854a75b87a
aws ec2 describe-instances --instance-id i-0ef8b75ced946121a
ssh -i "week2-lab3-key.pem" ec2-user@2600:1f18:6748:1400:3d8c:b5ee:443e:ca1b

aws ec2 create-security-group --group-name week2-lab3-private-subnet-SG --description "Security for week 2 lab 3 private subnet" --vpc-id vpc-09db7380650d26cdb
aws ec2 authorize-security-group-ingress --group-id sg-0de47ccbdb03b755a --ip-permissions '[{"IpProtocol": "tcp", "FromPort": 22, "ToPort": 22, "Ipv6Ranges": [{"CidrIpv6": "2600:1f18:6748:1400::/64"}]}]'
aws ec2 authorize-security-group-ingress --group-id sg-0de47ccbdb03b755a --ip-permissions '[{"IpProtocol": "58", "FromPort": -1, "ToPort": -1, "Ipv6Ranges": [{"CidrIpv6": "::/0"}]}]'
aws ec2 run-instances --image-id ami-a4827dc9 --count 1 --instance-type t2.micro --key-name week2-lab3-key --security-group-ids sg-0de47ccbdb03b755a --subnet-id subnet-0b07a5a5c6e0125ad
aws ec2 describe-instances --instance-id i-0ad1b951120bb97c7

 aws ec2 terminate-instances --instance-ids i-0ad1b951120bb97c7
 aws ec2 terminate-instances --instance-id i-0ef8b75ced946121a
  aws ec2 delete-security-group --group-id sg-0de47ccbdb03b755a
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-security-group --group-id sg-0bd13ca6c5e577e74
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-subnet --subnet-id subnet-0b07a5a5c6e0125ad
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-subnet --subnet-id subnet-0d05494854a75b87a
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-route-table --route-table-id rtb-085942b2d344ec0ce
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-route-table --route-table-id rtb-080f4e578dc2edb43
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 detach-internet-gateway --internet-gateway-id igw-02d72b194d4496f20 --vpc-id vpc-09db7380650d26cdb
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-internet-gateway --internet-gateway-id igw-02d72b194d4496f20
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-egress-only-internet-gateway --egress-only-internet-gateway-id eigw-05ba28e8b004f65fe
{
    "ReturnCode": true
}
[cloudshell-user@ip-10-1-56-101 ~]$ aws ec2 delete-vpc --vpc-id vpc-09db7380650d26cdb