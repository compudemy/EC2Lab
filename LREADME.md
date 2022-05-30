# EC2Lab
How to create EC2 Instances and launch them

Now you know the basics of the EC2 service and know that there are a lot of options and things to do. But the most important thing is to apply the knowledge. So let’s see how to create EC2 instances and launch them using an AWS Marketplace AMI so you can see the complete process to create EC2 Instance.

This tutorial will show you how to create EC2 instances from scratch, but if you already have a WordPress site to migrate from cPanel you can check this tutorial.

Requirements:
1 x AWS account (if it’s an IAM user verify that you have permission to launch EC2 instances)

Instructions:

Step 1
First of all, we need to login into the AWS Console, access using this URL: https://aws.amazon.com/console/and you are going to see this page, click the button “Sign In to the Console.”

![image](https://user-images.githubusercontent.com/103466963/171038570-f054c2a0-8cfd-46f8-a647-0fe6558cef4d.png)

Step 2
It’s going to open a page requesting your username or email. Type your password.

![image](https://user-images.githubusercontent.com/103466963/171038657-2e0d9849-087c-489e-b1d5-af64f0448627.png)

Step 3
This it’s what the AWS console looks like:

![image](https://user-images.githubusercontent.com/103466963/171038756-e3121973-ae6a-4626-8ac0-dd7748bdb73c.png)

![image](https://user-images.githubusercontent.com/103466963/171038849-6ab7cafb-bd50-4695-9e6f-83ff65338605.png)

Step 4
Click on EC2.

![image](https://user-images.githubusercontent.com/103466963/171038893-2d16ea4d-23d2-4481-873b-8a0abf798cf1.png)

Step 5
This is how the EC2 Dashboard looks.

![image](https://user-images.githubusercontent.com/103466963/171038997-39e1fd9e-5ced-4b40-97ce-b4e16b3c461d.png)

Step 6
Click the big blue button “Launch Instance.”

![image](https://user-images.githubusercontent.com/103466963/171039051-f3af599b-6a8a-4843-b2b3-130061df28cc.png)

Step 7
Immediately after you click the button a list of AMIs is going to appear. There you can select the one that you prefer, in this case, I’m going to choose Ubuntu Server 16.04 LTS.

Step 8
After you click the select button of your chosen AMI, you are going to be able to select the EC2 instance type from the list, for this example, I’m going to use t2.micro, select and the next button.

![image](https://user-images.githubusercontent.com/103466963/171039163-22414e73-bc08-4a88-87b8-31d140b3ce07.png)

Step 9
The next page is going to show you some options to set up your instance. For the moment, just be sure that the Auto-assign Public IP it’s set to enable and click next.

![image](https://user-images.githubusercontent.com/103466963/171039203-f8b114b2-fa82-4d15-95e9-37ff5e2789c6.png)

Step 10
Now, on this page, you can choose the amount of GB that your hard drive it’s going to have.

Note: Never leave the default 8gb, if you want to be on the free tier limits you can set a value around 20gb -24gb, because sometimes you leave it as default and your instance is not going to have too many spaces to do many things, and click next.

![image](https://user-images.githubusercontent.com/103466963/171039288-ce960a42-4606-4f35-a83d-80bce91a1a4c.png)

![image](https://user-images.githubusercontent.com/103466963/171039400-03b44ddd-90e9-4bb1-8abe-7c2f3dd1c038.png)

Step 11
You should be on the Tags screen.

![image](https://user-images.githubusercontent.com/103466963/171039543-8bb7d3b2-a184-4926-955c-92cc9134447c.png)

![image](https://user-images.githubusercontent.com/103466963/171039580-49ac3d20-c7db-487d-9222-ba0047a6e65c.png)

Step 12
Let’s click on the button “Add Tag” and on the key column type “Name” and on the value type the name that you want to give to your instance and click on next.

![image](https://user-images.githubusercontent.com/103466963/171039732-cbc81d11-6e97-46be-9067-ea7c07947dd9.png)

Step 13
Setup a Security Group: A Security group it’s like the firewall for your instance. Here you have to open the ports that you are going to use: i.e., if you want to have a web server you need to open the port 80 if you want ssh access you need the port 22, so let’s create a new one.

![image](https://user-images.githubusercontent.com/103466963/171039854-0f057db3-be71-4878-b3fd-4037903ae051.png)

Step 14
Let’s give a name and description to the new Security Group.

![image](https://user-images.githubusercontent.com/103466963/171039936-59345411-f7f0-414d-8c44-275bb14a4c66.png)

Step 15
Ports: The port 22 (ssh) open world (0.0.0.0/0) is added by default; we can leave it like that or choose “my IP” on the list of the source column. As a security measure, AWS only open ssh port to the ones that you trust and don’t leave it open world, add a description to identify and click on “Review and Launch.”

![image](https://user-images.githubusercontent.com/103466963/171039975-86f808b6-7a87-48fa-bce9-5aa02da2b47e.png)


Step 16
A Summary of all the setting that you have set it’s going to be shown on screen if everything looks fine, click on Launch, if not you can return to change everything that you need.

![image](https://user-images.githubusercontent.com/103466963/171040015-0c83f61d-54f1-4f55-b5a3-4edf993c751b.png)


Step 17
A key? This little thing is what you need to be able to access your brand new instance. Give it a name and click the button “Download Key Pair.” Then, save that file because you are not going to be able to get it in another time. AWS doesn’t store keys, so if you lose it, you are going to lose access to your instance.

Step 18
Click on “Launch Instances.”

![image](https://user-images.githubusercontent.com/103466963/171040056-ccbf71e8-e7af-4f7f-a643-dc9508c27aaa.png)


If everything goes well you are going to see the next screen, click on the id to view it on the dashboard.

![image](https://user-images.githubusercontent.com/103466963/171040100-1a778842-8b03-495e-8e0f-f75499e469ff.png)



