# Access-S3-from-VPC
Documentation on how to access S3 from a VPC.

In this project we are going to learn the following:
- S3
- EC2
- VPC
- Public Subnet
- Security Group
- Accessing and managing S3 from EC2


## AWS Architecture Diagram:

<img width="704" height="424" alt="Screen Shot 2025-09-15 at 10 55 15 am" src="https://github.com/user-attachments/assets/7df71892-9cba-49ef-9503-059a5c1091bd" />

## VPC and Public Subnet
Let's first create a VPC with a public subnet using the resource map in AWS.

Steps:
1. Go to VPC console window and select "Your VPCs".
2. Click "Create VPC" on the top right corner.
3. Select "VPC and more".
4. Setup the following as below and click "Create VPC". 
  <img width="1920" height="1827" alt="screencapture-ap-southeast-2-console-aws-amazon-vpcconsole-home-2025-09-15-11_07_28" src="https://github.com/user-attachments/assets/77c8dbdb-1249-4981-9b7e-f5fa966dad8c" />
Note: This should setup the VPC with all its required components such as: Internet Gateway, NACLs, Security Groups and Route Tables.


5. Click "View VPC".
6. This should display the following resource map implemented.
  <img width="1631" height="333" alt="Screen Shot 2025-09-15 at 11 10 21 am" src="https://github.com/user-attachments/assets/37af407b-423e-4b70-884f-0f7ac179490e" />

Now that we have the VPC and Public Subnet implemented, we need to launch an EC2 instance in this public subnet.


## Launching EC2 instance in the public subnet
1. Go to EC2 console window.
2. Go to "Instances" in the left navigation bar.
3. Click on "Launch instances" on the top right.
4. Keep the default selections. Only need to edit the Network Settings as below and click "Launch Instance".
   <img width="1920" height="3027" alt="screencapture-ap-southeast-2-console-aws-amazon-ec2-home-2025-09-15-11_18_30" src="https://github.com/user-attachments/assets/a5fb3c08-3885-4f19-b14a-8700632ff819" />

5. Now connect to the instance using Instance Connect:
   <img width="1664" height="612" alt="Screen Shot 2025-09-15 at 11 21 06 am" src="https://github.com/user-attachments/assets/645962ba-713a-4968-af2e-caaf5d10e247" />

6. Now type in the command: aws s3 ls
7. This should spit out the following error: Unable to locate credentials.
   <img width="802" height="274" alt="Screen Shot 2025-09-15 at 11 25 47 am" src="https://github.com/user-attachments/assets/065eefff-a35e-4a23-b7d9-b38ec3459125" />

Now we need to setup credentaials for the EC2 instance for it to be able to access and manage the S3 resource.


## AWS IAM
Now let us setup the access credentials for the EC2 instance. 
Note: For the scope of this project, we are using access credentials. The recommended method would be to create an IAM role and attach it to the EC2 instance. 

1. Go to IAM console window.
2. Search for 'access keys' on the left navigation:
   <img width="817" height="318" alt="Screen Shot 2025-09-15 at 11 31 23 am" src="https://github.com/user-attachments/assets/9e8579e6-bda8-41d1-957d-5308fd12eeed" />

3. Select create access keys: 
  <img width="1375" height="281" alt="Screen Shot 2025-09-15 at 11 32 02 am" src="https://github.com/user-attachments/assets/435b106a-8a92-4186-9d12-28c37dd159fb" />

4. Select 'Command Line Interface' and check the confirmation checkbox below and click 'Next'.

  <img width="1446" height="893" alt="Screen Shot 2025-09-15 at 11 32 45 am" src="https://github.com/user-attachments/assets/bcdcc726-eaf9-4fd7-bb81-6a59ba76cc96" />

5. Add a description tag - optional.
6. Click 'Create Access Key':
  <img width="1375" height="306" alt="Screen Shot 2025-09-15 at 11 34 33 am" src="https://github.com/user-attachments/assets/ab3199ca-df71-4230-806f-6c8177e455de" />

7. The secret key shows only on this page and never again. This can be saved for later use or a new one can be created.

Now head to the EC2 terminal window and use these keys to configure the instance.

## Configure EC2 instance

1. Use the command 'aws configure' to setup the access credentials.
2. Use the region name from here:

   <img width="384" height="579" alt="Screen Shot 2025-09-15 at 11 38 07 am" src="https://github.com/user-attachments/assets/805d5f8a-1a01-487e-8e11-4f03fc6b8eda" />
   
4. Keep it empty for the 'Default output format'.
5. Now try the command: aws s3 ls.

   This should now show a list of all buckets in my account under S3:

   <img width="495" height="142" alt="Screen Shot 2025-09-15 at 11 40 20 am" src="https://github.com/user-attachments/assets/cc8fefa3-96d0-496d-9519-86300bcf8348" />


## Manage S3 from EC2

In this step we are going to create a bucket using S3 console and then create and upload a content into the S3 bucket using EC2 instance.

1. Create a bucket in S3.

   <img width="1920" height="2139" alt="screencapture-ap-southeast-2-console-aws-amazon-s3-bucket-create-2025-09-15-11_44_46" src="https://github.com/user-attachments/assets/948f28b0-6467-4620-abe6-b27e238f58f5" />

2. Go to EC2 terminal window.
3. Use command - sudo touch /tmp/test.txt - to create a text file in tmp folder.
4. Use command - aws s3 cp /tmp/test.txt s3://demo-project-debashish - to copy the text file into the new bucket we created.
5. Use command - aws s3 ls s3://demo-project-debashish - to look into the contents in this bucket.

   <img width="741" height="110" alt="Screen Shot 2025-09-15 at 11 50 45 am" src="https://github.com/user-attachments/assets/202b1336-6612-4446-803d-ecd5d4ad9e0f" />

We can see, using EC2 terminal, that the text file has been moved to the S3 bucket.

Let's confirm this in the S3 console:

<img width="1669" height="341" alt="Screen Shot 2025-09-15 at 11 52 15 am" src="https://github.com/user-attachments/assets/a558aba8-cd7a-42c6-a697-b1340c8e1dfb" />


### Therefore, our implemented architecture is now working as expected.

<img width="704" height="424" alt="Screen Shot 2025-09-15 at 10 55 15 am" src="https://github.com/user-attachments/assets/7df71892-9cba-49ef-9503-059a5c1091bd" />


### Delete all the resources

1. Delete the access credentials.
2. Delete S3 (first the contents, then the bucket itself).
3. Terminate the EC2 instance.
4. Delete the VPC (this should delete all the associated componentes e.g. IG, NACLS and Security Groups).
   



   
  

   
   
