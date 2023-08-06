**Amazon Disaster Recovery - Basic Setup**

**Use below link to download the source code to start**

https://github.com/kohlidevops/aws-disaster-recovery/tree/main

**Setup DynamoDB table and Lambda**

I'm using Mumbai region for my setup. As of now, it should be my Primary region.

Navigate to DynamoDB and create a new table.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/968c619f-2131-4131-acda-50fdea14ed3e)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/7257cf9e-fb87-4f64-b3e5-a5550067a111)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/af11c35e-fdb3-4688-a5f2-3e894759b8c6)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/974c370b-6149-478d-b67a-2acfec1b525a)

Let's go ahead to create a DynamoDB table

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/433420a2-f514-4e0d-af69-7adf1e54bdfe)

**To create a IAM Role for Lambda**

Navigate to IAM console – select – create a new Role.

Trusted entity – Lambda and below policies should be associated.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/fbb4d5d2-72f4-442e-a1af-4f14433e0a13)

**To create a Lambda Function**

To download the zip file for Lambda Function.

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/Lambda/productDynamoDB.zip

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/d8b344a0-b517-4c8b-89b6-0b770d913f71)

Let’s go ahead to create a function.

Now upload the productDynamoDB.zip from local to lambda function and save a function

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/b903d236-f4d3-4ff2-9967-7809beffc242)

Edit the run time and modify the lambda handler name

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/9ebb2b68-225a-4bd4-9ef6-c1a763a0e142)

Handler name should be – lambdafunctionname.lambda_handler

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/de2f7232-f772-4b4c-802f-64fa20f53803)

For my case, productDynamoDB.lambda_handler

Then download the below Payload test event file for Lambda function. Just copy/paste in test event and save. Lets go ahead to test and add a products in a product table

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/Lambda/AddProductEvent.txt

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/3d63e0fa-f997-4d7c-915c-03ab47905dc8)

Here we go, the test is succeeded.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/8532f4b9-3a7b-4758-b917-bef927e7f393)

**To check the DynamoDB table**

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/25b0d1bf-2a79-4912-892a-4d84c4b1bc9e)

Select the table – Explore table names.

**Create an API**

To create a HTTP API – Select - Import

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/4a579294-c3bd-4cfa-aa7c-6d63d2e85df0)

Download the file using below link then copy/paste the code.

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/APIGW/APIGatewayProductAPI.json

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/af8b9a1d-8f4f-48f7-9a91-8b1c78ba3954)

Let's go ahead to create an API.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/446bdd61-42c9-4251-b404-f9dbcbc84bea)

Change integration – update the region and your lambda function which is created now.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/45c75bb7-9c5d-4b51-a9c3-9da8a3237a51)

Do edit and update the region and lambda function for all integrations.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/4523f60f-383b-4d89-bf16-9d6783241ad8)

Let's deploy the API. So first create a stage and deploy the API.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/4c5c3ecb-d311-41cb-84fa-409845795926)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/b4e317b4-4fd0-4938-8a51-6dbcd50b6249)

Let's go ahead to create stage as Mumbai. Then deploy this stage.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/73c91518-5fca-4cfe-9887-0f6622705887)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/c1332d53-7ca4-4059-b712-7a4a99b681d3)

After deployed, test with API url.

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/de7d1ff9-a325-4072-8744-24ec744503c8)

https://zvr0bj71oi.execute-api.ap-south-1.amazonaws.com/mumbai/products

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/29d2c944-3b30-4c23-a7f7-96e3790475e8)

These items really come from DynamoDB.

We are calling API URL, this API integrates with lambda function, so this API calling lambda function. Then this lambda function will fetch the table values from DynamoDB.

**To launch instance for web server**

Before create 3 security group – EC2 SG (SSH/HTTP), ALB SG (HTTP) and NullSG (No inbound/outbound).

Launch AMZ Linux2 Ec2 instance and associate DR-EC2SG, then add user data using below link.

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/WebPage/UserData.txt

Then let’s go ahead to launch ec2 instance & SSH to machine and update the below API URL in /var/www/html/index.html

Update and save the API URl like below.

 ![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/ed2f5019-13a8-407a-b7cb-89f1fb569a37)

 Then add the Mumbai in Products Editor line then save.

 ![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/4efb407b-101c-46fe-919d-f48d11e17d0a)

**To create a Target Group in AWS**

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/09e8eae7-fb01-4672-86d6-b22e553d60eb)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/84c5a5ca-6837-4966-8df7-ad2c7985be2d)

**To create a Application Loadbalancer**

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/d32be115-7727-41d3-a122-bc270045e1b5)
![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/f9eb9fe9-0bb1-4e9e-b003-10f2e7e2c55a)

To attach security group and associate Target group in listener rule

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/f34df79b-59b6-4bf2-b7f5-65cf3c34f3ab)

Let’s go ahead to create an Application Load balancer.

**To create a record in AWS R53**

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/381adb76-2e07-44e7-abaa-72504c762bfd)

After subdomain creation

![image](https://github.com/kohlidevops/aws-disaster-recovery/assets/100069489/da95b828-4065-4b50-acc1-ed9899ec5d02)

Basic setup has been completed and ready to go.

















