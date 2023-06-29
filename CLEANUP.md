To make sure you do not incur additional charges when you no longer need the to experiment with the jumpstart model, follow these steps to remove the resources you created when following the blog post [Easily access private repos using the remote decorator mode for SageMaker training workloads]().

### 1. Make note of the Amazon Resource Name (ARN) for the SageMaker Studio domain
You will need the full ARN of the SageMaker Studio domain in step 2.   
- Go to the CloudFormation console, select the second stack you created (named `SageMaker-Studio-CodeArtifact`, if you used the suggested name in the blog). 
- Go to the Output tab, copy the value of the output named `StudioDomainArn` and keep it somewhere safe.

### 2. Delete the KernelGateway App which is running the notebook. 
Read [Shut Down Resources](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks-run-and-manage-shut-down.html) for detailed steps. Wait for the app to be deleted.

### 3. Delete the SageMaker Studio and associated resources
- Go to the CloudFormation console.
- Locate the second stack you created (named `SageMaker-Studio-CodeArtifact`, if you used the suggested name in the blog) and delete the stack.
- When the stack is deleted, move to the next step.

### 4. Remove the mount points (ENIs) for the EFS volume, ENI for the inference endpoint, and security groups created by SageMaker Studio
In this step, you will delete the resources that were automatically created when you created the SageMaker Studio domain, including the EFS mount points and security groups.

- Go to [clean-up](clean-up/) folder. 
- Make sure the dependencies are installed by running:
    - `python3 -m pip install --user -r requirements.txt`
- Run `cleanup.py`, passing in the the ARN of the SageMaker Studio domain as an argument.
    - `python3 cleanup.py --sagemaker-studio-domain [ARN for the SageMaker Studio domain from step 1]`

The script does not delete the volume to prevent accidental data loss. We ask you to delete the EFS volume yourself once you have made sure you do not need any data or code you might have created in SageMaker Studio. The script writes the File System ID for the EFS volume. Make a note of the file system ID as you will need it in step 6.

### 5. Delete the VPC and other networking resources
You can now delete the VPC and its associated resources such as VPC endpoints and subnets. In the CloudFormation console, locate the first stack you created (named `No-Internet`, if you used the suggested name) and delete it. If the stack fails to delete after a while, make sure you have completed step 4.

### 6. Delete the EFS volume
Once you have made sure you do not need the data in the EFS volume, go to EFS console, locate the EFS volume using the file system ID you noted earlier, and delete it.
