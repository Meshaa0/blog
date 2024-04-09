+++
author = "TechMesha"
title = "Game Deployment: CI/CD Pipeline from GitHub to AWS S3"
date = "2024-04-04"
description = "Game Deployment: CI/CD Pipeline from GitHub to AWS S3"
tags = [
    "S3",
    "GitHub",
    "CI/CD",
]
+++

CI/CD stands for Continuous Integration and Continuous Deployment is a simplified process that automates the different stages, such as building, testing and deploying, of developing software. As software is developed using CI/CD, developers guarantee that it is continuously left in a deployable condition. Changes in the code are integrated safely and as soon as feasible in this case in a practical setting. This development methodology shortens the development cycle while increasing the quality of the program in question by allowing for quick and often deployment.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/q9wCMWF/React-App-with-Amplify-Cognito-and-CI-CD-with-Git-Hub.png" alt="React-App-with-Amplify-Cognito-and-CI-CD-with-Git-Hub" border="0"></a><br />

## Technologies Used

1. **GitHub:** A versatile platform for version control and collaboration, facilitating code management and team cooperation.
2. **Amazon S3:** A scalable cloud storage service by AWS, enabling secure data storage, website hosting, and file management.
3. **AWS CodePipeline:** An automated CI/CD service orchestrating software delivery stages for streamlined development processes.

## 01: Create an S3 Bucket

To begin hosting your static website, follow these steps:

1. Navigate to the Amazon S3 section of the AWS Management Console https://console.aws.amazon.com/s3/
2. Click "Create bucket".
3. You should choose a unique name for your bucket.
4. Select the desired region for your bucket.
5. In the section concerning public access settings, ensure the box labeled "Block all public access" is unchecked.
6. Maintain default settings and create the bucket by clicking "Create".

<a href="https://ibb.co/xGgjtwJ"><img src="https://i.ibb.co/TR08xJm/Screenshot-from-2024-04-03-17-58-45.png" alt="Screenshot-from-2024-04-03-17-58-45" border="0"></a> 

**<center> . . .</center>**

## 02: Host Using S3 Bucket 

1. Access your created bucket and navigate to "Properties".
2. Under "Static website hosting", click "edit".
3. Choose "Enable" for Static website hosting.
4. Specify the names of your index.html and error.html files.
5. Maintain existing settings and click "Save changes" to finalize.

<a href="https://ibb.co/xGgjtwJ"><img src="https://i.ibb.co/TR08xJm/Screenshot-from-2024-04-03-17-58-45.png" alt="Screenshot-from-2024-04-03-17-58-45" border="0"></a>

<a href="https://ibb.co/gFFV32Z"><img src="https://i.ibb.co/tPPX4Gp/Screenshot-from-2024-04-03-18-00-55.png" alt="Screenshot-from-2024-04-03-18-00-55" border="0"></a>

**<center> . . .</center>**

## 03: Grant Public Access

1. Open bucket settings and locate "Permissions".
2. Click on "Bucket Policy".
3. Use this code for your bucket policy. This allows everyone to read/view everything in the bucket. Be sure to update the Bucket-Name in the policy. 

<a href="https://ibb.co/X5FPX3m"><img src="https://i.ibb.co/M1PHnGq/Screenshot-from-2024-04-03-18-02-28.png" alt="Screenshot-from-2024-04-03-18-02-28" border="0"></a>

Here is the code if you want to copy past 

{{< highlight html >}}
{
    "Version": "2012-10-17",
    "Statement": [
    	{
        	"Sid": "PublicReadGetObject",
        	"Effect": "Allow",
        	"Principal": "*",
        	"Action": [
            	"s3:GetObject"
        	],
        	"Resource": [
                "arn:aws:s3:::Bucket-Name/*"
        	]
    	}
    ]
}
{{< /highlight >}}

**<center> . . .</center>**

## Step 4: Integrating AWS CodePipeline with GitHub

1. Navigate to the AWS console and search for CodePipeline.

2. Click "Create Pipeline" and assign a name. Then, proceed to the next step.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/fdxqH38/Screenshot-from-2024-04-03-18-14-43.png" alt="Screenshot-from-2024-04-03-18-14-43" border="0"></a>

To link CodePipeline with GitHub:

-  Access the Developer Tools console at https://console.aws.amazon.com/codesuite/settings/connections
-  Choose "GitHub" under "Select a provider".
-  Click "Connect to GitHub" and follow the provided instructions.

4. After connecting GitHub, return to CodePipeline and:

- Select "GitHub (Version 2)" under "Source provider".
- Choose the GitHub connection created earlier from the dropdown menu.
- Specify the repository containing your project's source code.
- Select the desired branch for the pipeline.
- Proceed to the next step.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/3dp9Cjm/Screenshot-from-2024-04-03-18-16-00.png" alt="Screenshot-from-2024-04-03-18-16-00" border="0"></a>

<a href="https://ibb.co/jTv9WLq"><img src="https://i.ibb.co/G0vftVG/Screenshot-from-2024-04-03-18-19-10.png" alt="Screenshot-from-2024-04-03-18-19-10" border="0"></a>

5. For building, opt hit "skip build stage"

<a href="https://ibb.co/cgngD8k"><img src="https://i.ibb.co/FxTxgYm/Screenshot-from-2024-04-03-18-19-58.png" alt="Screenshot-from-2024-04-03-18-19-58" border="0"></a>

6. In the Deploy section, select the deployment target, such as an Amazon EC2 instance, Amazon ECS service, or Amazon S3 bucket.

<a href="https://ibb.co/kgtcrpD"><img src="https://i.ibb.co/KjJyQYs/Screenshot-from-2024-04-03-18-20-36.png" alt="Screenshot-from-2024-04-03-18-20-36" border="0"></a>


and now click "Create" to establish the CI/CD pipeline.

Now the CI/CD pipeline will promptly initiate builds upon any alterations to the source code within your GitHub repository. Subsequently, the resulting build artifacts will be seamlessly deployed to the designated deployment target, which in this case is the specified S3 Bucket.

<a href="https://ibb.co/RhjtyH2"><img src="https://i.ibb.co/M8BK6Nn/Screenshot-from-2024-04-03-18-35-35.png" alt="Screenshot-from-2024-04-03-18-35-35" border="0"></a>

**<center> . . .</center>**

Lastly, to view our deployed game, open a new browser tab and navigate to the AWS S3 section.

Find your designated S3 bucket and click on its name to access its settings. Within the bucket, locate and click on "Properties". Scroll down to the "Static website hosting" section.

Here, you'll find a link labeled as the Bucket website endpoint. Click on this link to seamlessly explore our hosted game and access it publicly. 

<a href="https://ibb.co/256srxP"><img src="https://i.ibb.co/M9hGHrp/Screenshot-from-2024-04-03-18-37-07.png" alt="Screenshot-from-2024-04-03-18-37-07" border="0"></a>

**<center> . . .</center>**

Also, you can test your CI/CD pipeline by modifying your code files and committing them to your GitHub repository.

<a href="https://ibb.co/8Ds3946"><img src="https://i.ibb.co/fYFRHG4/Screenshot-from-2024-04-03-19-51-22.png" alt="Screenshot-from-2024-04-03-19-51-22" border="0"></a>

**<center> . . .</center>**

## Final Thoughts

**In summary,** we've built a robust CI/CD pipeline utilizing AWS tools. By integrating GitHub, AWS CodePipeline, and Amazon S3, we've automated and optimized every stage of our game development process.




