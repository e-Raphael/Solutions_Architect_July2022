aws ec2 create-placement-group --group-name Schull-PG --strategy partition --partition-count 4
aws ec2 create-tags --resources pg-0b8cc39bed3c3b623 --tags Key=Name,Value=partition-group-for-schull
aws ec2 describe-tags --filters Name=resource-id,Values=pg-0b8cc39bed3c3b623
aws ec2 run-instances --image-id ami-05fa00d4c63e32376 --instance-type t2.micro --subnet-id subnet-0b9cba12e6bd3937d --security-group-ids sg-07174d20e2381f176 --associate-public-ip-address --key-name schull-key --placement "GroupName = Schull-PG, PartitionNumber = 2"
aws ec2 describe-instances --instance-id i-001198d0ac8d4701e
aws ec2 create-placement-group --group-name Schull-PG-3 --strategy partition --partition-count 6
aws ec2 stop-instances --instance-id i-001198d0ac8d4701e
aws ec2 modify-instance-placement --instance-id i-001198d0ac8d4701e --group-name Schull-PG-3
aws ec2 describe-instances --instance-id i-001198d0ac8d4701e
aws ec2 start-instances --instance-id i-001198d0ac8d4701e
aws ec2 terminate-instances --instance-id i-001198d0ac8d4701e
aws ec2 delete-placement-group --group-name Schull-PG-3
aws ec2 delete-placement-group --group-name Schull-PG