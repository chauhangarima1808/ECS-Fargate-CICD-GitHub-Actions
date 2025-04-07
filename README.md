# ECS-Fargate-CICD-GitHub-Actions

This project involved setting up an automated containerized application on Amazon ECS Fargate using Terraform, with Docker images pushed to Amazon ECR. I implemented secure private networking, Blue-Green deployments, and encrypted logging via CloudWatch and KMS.
## Project Highlights
- Provision ECS Fargate tasks within private subnets using Terraform to enhance security and isolation.
- Configure VPC Interface Endpoints for Amazon ECR and Amazon S3 to ensure all data traffic remains within the AWS network, reducing exposure to the public internet.
- Build and containerize the application using Docker, and securely push the image to Amazon Elastic Container Registry (ECR).
- Deploy the application to Amazon ECS services , with integrated health checks and enable centralized monitoring through Amazon CloudWatch Logs.
- Implement AWS Key Management Service (KMS) encryption , to secure CloudWatch log data and ECR image layers at rest.
- Integrate AWS CodeDeploy to orchestrate Blue-Green deployments , on ECS Fargate, facilitating seamless and controlled traffic shifting between application environments.
- Leverage ECS Fargate’s serverless container platform , to eliminate infrastructure management overhead and optimize costs through a pay-per-use pricing model.

## Architecture
### Target technology stack 
- [Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)
    - [SageMaker Pipelines](https://aws.amazon.com/sagemaker/pipelines/)
    - [SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)
- [Amazon Simple Storage Service (S3)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
### Target architecture
![Architecture Diagram](/architecture-diagram.png "Architecture Diagram")
### Automation and scale
After registering the trained model to SageMaker Model Registry, you can choose to deploy the model to a SageMaker endpoint for real-time inference. For more information about this, see [Deploy a Model](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry-deploy.html) from the Registry in the Amazon SageMaker documentation.
## Tools
- [Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) — SageMaker is a fully managed ML service
- [Amazon SageMaker Pipelines](https://aws.amazon.com/sagemaker/pipelines/) — SageMaker Pipelines help create, automate, and manage end-to-end ML workflows at scale
- [Amazon SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html) — SageMaker Model Registry helps centrally catalog and manage trained ML models
- [Amazon Simple Storage Service (Amazon S3)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) — Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance.
- [Python](https://www.python.org/) — Python is a programming language.
## Getting started
### Step 1. Prepare Amazon S3 Bucket for image dataset
- Create a new S3 bucket with default settings via the Amazon S3 console
- Create a new folder named “ImageData” within the newly created S3 bucket
    - Within the “ImageData” folder, create two subfolders named “Pizza” and “NotPizza”.
- Locally download [Pizza or Not Pizza?](https://www.kaggle.com/datasets/carlosrunner/pizza-not-pizza) dataset on to your computer and unzip its contents
    - You should notice two subdirectories within the downloaded file: (1) “pizza” and (2) “not_pizza”
    - **Note:** You may have to create a free account with Kaggle.com to access the dataset.
- Navigate to the “Pizza” folder in the S3 bucket and upload 500 randomly selected images from the “pizza” subdirectory from the locally downloaded dataset.
- Navigate to the “NotPizza” folder in the S3 bucket and upload 500 randomly selected images from the “not_pizza” subdirectory from the locally downloaded dataset.
### Step 2. Configure Amazon SageMaker Studio environment
- Create a new SageMaker Domain and User Profile via the Amazon SageMaker console
    - Follow the instructions from [Onboard to Amazon SageMaker Domain Using Quick setup](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html) from the Amazon SageMaker documentation.
    - **Note:** When setting up the IAM role for the user profile, ensure that you give access to the Amazon S3 bucket you created earlier.
    - **Note:** Ensure that the SageMaker Domain is created in the same AWS Region as the S3 bucket you created earlier
- Launch SageMaker Studio application via the User 
    - Follow the instructions from [Launch Studio Using the Amazon SageMaker Console](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-launch.html#studio-launch-console) from the Amazon SageMaker documentation
- Download the `image-classification-sagemaker-pipelines.ipynb` Jupyter notebook and `scripts` folder from this GitHub repository
- Upload the `image-classification-sagemaker-pipelines.ipynb` Jupyter notebook and `scripts` folder to the SageMaker Studio application
### Step 3. Run the Jupyter notebook in Amazon SageMaker Studio
- Sequentially run the code cells from the `image-classification-sagemaker-pipelines.ipynb` Jupyter notebook within SageMaker Studio
    - **Note:** Make sure to appropriately configure the `TODO` portions of the code as you run the code cells
- You can graphically monitor the pipeline execution in SageMaker Studio.
    - Follow the instructions from [View a Pipeline](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-studio-list-pipelines.html) from the Amazon SageMaker documentation.
- After the pipeline is finished, you can view the registered model and associated metadata within SageMaker Studio.
    - Follow the instructions from [View the Details of a Model Version (Amazon SageMaker Studio)](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry-details.html#model-registry-details-studio) section of Amazon SageMaker documentation.
## Clean up
1. Delete the S3 bucket with the image dataset and the default S3 bucket created by the SageMaker session
    - For more information on this, follow the instructions from [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html) from the Amazon S3 documentation.
2. Delete the default S3 bucket created by the SageMaker session
    - **Note:** The default S3 bucket created by the SageMaker session should be in the following format: *"sagemaker-{region}-{aws-account-id}”*
3. Delete model group from SageMaker Model Registry
     - Follow the instructions from [Delete a Model Group](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry-delete-model-group.htmleiifcbevrhk) from the Amazon SageMaker documentation.
     - **Note:** The model group name should be *“MXNet-Image-Classification.”* This was previously defined in the `image-classification-sagemaker-pipelines.ipynb` Jupyter notebook  
4. Delete SageMaker IAM execution role
    - First, navigate to your SageMaker Domain via the Amazon SageMaker console. Next, click on the "Domain Settings" tab. Now, under "General settings," you should find the "Execution role" for your SageMaker Domain. Copy the name of that "Execution role" (i.e. "AmazonSageMaker-ExecutionRole-XXXXX".
    - Next, navigate to the AWS IAM console and delete the SageMaker Execution (IAM) role you just copied. For more information about this, refer to [Deleting an IAM role (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html#roles-managingrole-deleting-console) from the AWS IAM documentation.
4. Delete SageMaker Domain
    - Follow the instructions from [Delete an Amazon SageMaker Domain (console)](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio) from the Amazon SageMaker documentation
## Security
See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.
## License
This library is licensed under the MIT-0 License. See the LICENSE file.
## Authors
- [Siddharth Kumaran](https://github.com/siddgood) -- Assoc. Machine Learning Engineer @ AWS Professional Services![image](https://github.com/user-attachments/assets/96addb92-d170-48e9-a2e9-8794f62f6044)

