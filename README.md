# EC2Lab

AWS EC2 Use Case
Next, in this AWS EC2 Tutorial, let’s understand the whole EC2 instance creation process through a use case in which we’ll be creating an Ubuntu instance for a test environment.

Login to AWS Management Console.

![image](https://user-images.githubusercontent.com/103466963/174836020-1e1bb9b1-2227-43a9-a21e-0bdf633d75d7.png)

2. Select your preferred Region. Select a region from the drop-down, the selection of the region can be done on the basis of the criteria discussed earlier in the blog.

![image](https://user-images.githubusercontent.com/103466963/174837374-d06bacfc-200f-4081-855b-dc68face4d4e.png)


3. Select EC2 Service Click EC2 under Compute section. This will take you to the EC2 dashboard.

![image](https://user-images.githubusercontent.com/103466963/174837454-90de9b31-5dc0-4a7e-a86f-1cb3484fe1c7.png)

4. Click Launch Instance.

5. Select an AMI: because you require a Linux instance, in the row for the basic 64-bit Ubuntu AMI, click Select.

![image](https://user-images.githubusercontent.com/103466963/174842309-3fc65ef4-b91b-4dca-b0d4-4f496db4e50a.png)

6. Choose an Instance

Select t2.micro instance, which is free tier eligible.

![image](https://user-images.githubusercontent.com/103466963/174842487-6477740b-4f48-4b29-8c01-959afa3f98dc.png)

7. Configure Instance Details.
Configure all the details and then click on add storage

![image](https://user-images.githubusercontent.com/103466963/174842869-2b229e5d-9e33-4475-9c0c-9a7405a8073f.png)

8. Add Storage

![image](https://user-images.githubusercontent.com/103466963/174843215-fac0e7fe-7a3c-42f2-be7f-46cc94b8234d.png)

9. Tag an Instance

Type a name for your AWS EC2 instance in the value box. This name, more correctly known as a tag, will appear in the console when the instance launches. It makes it easy to keep track of running machines in a complex environment. Use a name that you can easily recognize and remember.

![image](https://user-images.githubusercontent.com/103466963/174843817-740cd401-0a40-4c23-9eef-7c68e7e9f748.png)

10. Create a Security Group

![image](https://user-images.githubusercontent.com/103466963/174844047-e7c8acda-b94f-43b8-9439-37d7f7c9f407.png)

11. Review and Launch an Instance

Verify the details that you have configured to launch an instance.

![image](https://user-images.githubusercontent.com/103466963/174844252-a96c59fa-21c0-4ca6-a101-4b9c61923bf5.png)

12. Create a Key Pair & launch an Instance

Next, in this AWS EC2 Tutorial, select the option ‘Create a new key pair’ and give a name of a key pair. After that, download it in your system and save it for future use.

![image](https://user-images.githubusercontent.com/103466963/174844446-e43e1c25-4580-4a13-af35-97fb941a44fe.png)




**How to create an AWS EC2 instance using AWS CLI:**
Step 1: Get Amazon Linux 2 latest AMI ID.
## Get Amazon Linux 2 latest AMI ID
AWS_AMI_ID=$(aws ec2 describe-images \
--owners 'amazon' \
--filters 'Name=name,Values=amzn2-ami-hvm-2.0.????????-x86_64-gp2' 'Name=state,Values=available' \
--query 'sort_by(Images, &CreationDate)[-1].[ImageId]' \
--output 'text')
1
2
3
4
5
6
## Get Amazon Linux 2 latest AMI ID
AWS_AMI_ID=$(aws ec2 describe-images \
--owners 'amazon' \
--filters 'Name=name,Values=amzn2-ami-hvm-2.0.????????-x86_64-gp2' 'Name=state,Values=available' \
--query 'sort_by(Images, &CreationDate)[-1].[ImageId]' \
--output 'text')
Step 2: Create a key-pair.
## Create a key-pair
aws ec2 create-key-pair \
--key-name myvpc-keypair \
--query 'KeyMaterial' \
--output text > myvpc-keypair.pem
1
2
3
4
5
## Create a key-pair
aws ec2 create-key-pair \
--key-name myvpc-keypair \
--query 'KeyMaterial' \
--output text > myvpc-keypair.pem
Step 3: Create user data for a LAMP stack.
## Create user data for a LAMP stack
vi myuserdata.txt
-----------------------
#!/bin/bash
sudo yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install -y httpd mariadb-server
sudo systemctl start httpd
sudo systemctl is-enabled httpd
-----------------------
:wq
1
2
3
4
5
6
7
8
9
10
11
## Create user data for a LAMP stack
vi myuserdata.txt
-----------------------
#!/bin/bash
sudo yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install -y httpd mariadb-server
sudo systemctl start httpd
sudo systemctl is-enabled httpd
-----------------------
:wq
Step 4: Create an EC2 instance.
## Create an EC2 instance
AWS_EC2_INSTANCE_ID=$(aws ec2 run-instances \
--image-id $AWS_AMI_ID \
--instance-type t2.micro \
--key-name myvpc-keypair \
--monitoring "Enabled=false" \
--security-group-ids $AWS_CUSTOM_SECURITY_GROUP_ID \
--subnet-id $AWS_SUBNET_PUBLIC_ID \
--user-data file://myuserdata.txt \
--private-ip-address 10.0.1.10 \
--query 'Instances[0].InstanceId' \
--output text)
1
2
3
4
5
6
7
8
9
10
11
12
## Create an EC2 instance
AWS_EC2_INSTANCE_ID=$(aws ec2 run-instances \
--image-id $AWS_AMI_ID \
--instance-type t2.micro \
--key-name myvpc-keypair \
--monitoring "Enabled=false" \
--security-group-ids $AWS_CUSTOM_SECURITY_GROUP_ID \
--subnet-id $AWS_SUBNET_PUBLIC_ID \
--user-data file://myuserdata.txt \
--private-ip-address 10.0.1.10 \
--query 'Instances[0].InstanceId' \
--output text)
Step 5: Add a tag to the ec2 instance.
## Add a tag to the ec2 instance
aws ec2 create-tags \
--resources $AWS_EC2_INSTANCE_ID \
--tags "Key=Name,Value=myvpc-ec2-instance"
1
2
3
4
## Add a tag to the ec2 instance
aws ec2 create-tags \
--resources $AWS_EC2_INSTANCE_ID \
--tags "Key=Name,Value=myvpc-ec2-instance"
Step 6: Check if the instance is running.
## Check if the instance is running
aws ec2 describe-instance-status \
--instance-ids $AWS_EC2_INSTANCE_ID --output text
1
2
3
## Check if the instance is running
aws ec2 describe-instance-status \
--instance-ids $AWS_EC2_INSTANCE_ID --output text
Step 7: Get the public ip address of your instance.
## Get the public ip address of your instance
AWS_EC2_INSTANCE_PUBLIC_IP=$(aws ec2 describe-instances \
--query "Reservations[*].Instances[*].PublicIpAddress" \
--output=text) &&
echo $AWS_EC2_INSTANCE_PUBLIC_IP
1
2
3
4
5
## Get the public ip address of your instance
AWS_EC2_INSTANCE_PUBLIC_IP=$(aws ec2 describe-instances \
--query "Reservations[*].Instances[*].PublicIpAddress" \
--output=text) &&
echo $AWS_EC2_INSTANCE_PUBLIC_IP
Step 8: Try to connect to the instance.
## Try to connect to the instance
chmod 400 myvpc-keypair.pem
ssh -i myvpc-keypair.pem ec2-user@$AWS_EC2_INSTANCE_PUBLIC_IP
exit
1
2
3
4
## Try to connect to the instance
chmod 400 myvpc-keypair.pem
ssh -i myvpc-keypair.pem ec2-user@$AWS_EC2_INSTANCE_PUBLIC_IP
exit
Note: You can check your web server by opening your public IP in your favorite browser.

Cleanup:
Shell
## Cleanup
## Terminate the ec2 instance
aws ec2 terminate-instances \
--instance-ids $AWS_EC2_INSTANCE_ID &&
rm -f myuserdata.txt

## Delete key pair
aws ec2 delete-key-pair \
--key-name myvpc-keypair &&
rm -f myvpc-keypair.pem

## Delete custom security group
aws ec2 delete-security-group \
--group-id $AWS_CUSTOM_SECURITY_GROUP_ID

## Delete internet gateway
aws ec2 detach-internet-gateway \
--internet-gateway-id $AWS_INTERNET_GATEWAY_ID \
--vpc-id $AWS_VPC_ID &&
aws ec2 delete-internet-gateway \
--internet-gateway-id $AWS_INTERNET_GATEWAY_ID

## Delete the custom route table
aws ec2 disassociate-route-table \
--association-id $AWS_ROUTE_TABLE_ASSOID &&
aws ec2 delete-route-table \
--route-table-id $AWS_CUSTOM_ROUTE_TABLE_ID

## Delete the public subnet
aws ec2 delete-subnet \
--subnet-id $AWS_SUBNET_PUBLIC_ID

## Delete the vpc
aws ec2 delete-vpc \
--vpc-id $AWS_VPC_ID
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
## Cleanup
## Terminate the ec2 instance
aws ec2 terminate-instances \
--instance-ids $AWS_EC2_INSTANCE_ID &&
rm -f myuserdata.txt
 
## Delete key pair
aws ec2 delete-key-pair \
--key-name myvpc-keypair &&
rm -f myvpc-keypair.pem
 
## Delete custom security group
aws ec2 delete-security-group \
--group-id $AWS_CUSTOM_SECURITY_GROUP_ID
 
## Delete internet gateway
aws ec2 detach-internet-gateway \
--internet-gateway-id $AWS_INTERNET_GATEWAY_ID \
--vpc-id $AWS_VPC_ID &&
aws ec2 delete-internet-gateway \
--internet-gateway-id $AWS_INTERNET_GATEWAY_ID
 
## Delete the custom route table
aws ec2 disassociate-route-table \
--association-id $AWS_ROUTE_TABLE_ASSOID &&
aws ec2 delete-route-table \
--route-table-id $AWS_CUSTOM_ROUTE_TABLE_ID
 
## Delete the public subnet
aws ec2 delete-subnet \
--subnet-id $AWS_SUBNET_PUBLIC_ID
 
## Delete the vpc
aws ec2 delete-vpc \
--vpc-id $AWS_VPC_ID
Hope you get the idea of using AWS CLI. There is some limitation for using AWS CLI like passing output values of one code block to input value of another code block. Though this can be overcome by using variables like it has been done in the last two blogs. In my experience AWS CLI can be used for ad-hoc purpose. But if you want to build your infrastructure with DevOps methodology, SDK like Python Boto3 or external tools like terraform has much better options.

In the next blog post, we will start with a new AWS service. You can explore other AWS service related CLI using below link.
