aws s3api create-bucket --bucket raphael.demo 
aws s3api put-object --bucket raphael.demo --key pictures-folder/
aws s3api put-object --bucket raphael.demo --key pictures-folder/family1.jpg
aws s3api put-object --bucket raphael.demo --key pictures-folder/Dearest.jpg
aws s3api put-object --bucket raphael.demo --key pictures-folder/Bina1.jpg
aws s3api put-object --bucket raphael.demo --key pictures-folder/Bina2.jpg
aws s3api put-object --bucket raphael.demo --key pictures-folder/Bina3.jpg
aws s3api list-objects --bucket raphael.demo
aws s3api get-object --bucket raphael.demo --key pictures-folder/family1.jpg family1.jpg
aws s3api get-object --bucket raphael.demo --key pictures-folder/Dearest.jpg Dearest.jpg
aws s3api get-object --bucket raphael.demo --key pictures-folder/Bina1.jpg Bina1.jpg
aws s3api get-object --bucket raphael.demo --key pictures-folder/Bina2.jpg Bina2.jpg
aws s3api get-object --bucket raphael.demo --key pictures-folder/Bina2.jpg Bina3.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/family1.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/Dearest.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/Bina1.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/Bina2.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/Bina3.jpg
aws s3api delete-object --bucket raphael.demo --key pictures-folder/
aws s3api delete-bucket --bucket raphael.demo