## Setting Up AWS EC2 Using the AWS Management Console

Before using Terraform, you can manually set up an EC2 instance through the AWS Management Console. Here’s how:

### Step 1: Sign In to AWS Management Console

1. Go to the [AWS Management Console](https://aws.amazon.com/console/) and sign in with your credentials.

   ![AWS Console Sign In](https://d1.awsstatic.com/console/console_home.8f2952e9a0b63e1b6a5562b38309a55e.jpg)

   *Source: [AWS Console Sign In](https://aws.amazon.com/console/)*

### Step 2: Launch an EC2 Instance

1. In the AWS Management Console, navigate to **EC2** by selecting **Services** > **EC2**.

   ![EC2 Dashboard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/ec2-dashboard.png)

   *Source: [AWS EC2 Dashboard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)*

2. Click **Launch Instance** to start the instance creation wizard.

   ![Launch Instance](https://d1.awsstatic.com/ec2/Launch-Instance/EC2_Launch_Instance.png)

   *Source: [AWS Launch Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)*

3. Select an **Amazon Machine Image (AMI)**, such as Amazon Linux 2 AMI.

   ![Select AMI](https://d1.awsstatic.com/EC2/EC2_Amazon_Linux2_1.5.0.0.png)

   *Source: [AWS AMI Selection](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)*

4. Choose an **Instance Type**. For a free tier, select **t2.micro**.

   ![Select Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/choose-instance-type.png)

   *Source: [AWS Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)*

5. Configure **Instance Details**, **Add Storage**, and **Add Tags** as needed.

   ![Configure Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/configure-instance.png)

6. Configure a **Security Group** to allow SSH access (port 22).

   ![Configure Security Group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/security-group.png)

7. Review and **Launch** the instance. You’ll be prompted to select a key pair for SSH access.

   ![Launch Review](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/review-instance-launch.png)

8. Once launched, you can view your instance in the EC2 dashboard.

   ![EC2 Instances List](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/instance-list.png)

   *Source: [AWS EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)*

## Setting Up AWS EC2 Using Terraform

Terraform allows you to define your infrastructure as code. Here’s how to use Terraform to set up an EC2 instance:

### Prerequisites

1. **AWS Account**: Sign up at [AWS](https://aws.amazon.com/).
2. **Terraform**: Install it from [Terraform's website](https://www.terraform.io/downloads.html).
3. **AWS CLI**: Install and configure it.

### Steps to Set Up an EC2 Instance with Terraform

1. **Create a Terraform Configuration File**

   Create a file named `main.tf` with the following content:

   ```hcl
   # Define the provider (AWS in this case)
   provider "aws" {
     region = "us-east-1"  # Specify your preferred AWS region
   }

   # Define an EC2 instance resource
   resource "aws_instance" "my_instance" {
     ami           = "ami-0c55b159cbfafe1f0"  # Amazon Linux 2 AMI (update as needed)
     instance_type = "t2.micro"                # Free tier instance type

     # Add tags to your instance
     tags = {
       Name = "MyTerraformInstance"
     }

     # Optional: Define a security group inline
     vpc_security_group_ids = [aws_security_group.sg.id]
   }

   # Define a security group
   resource "aws_security_group" "sg" {
     name_prefix = "terraform-sg-"

     ingress {
       from_port   = 22
       to_port     = 22
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }

     egress {
       from_port   = 0
       to_port     = 0
       protocol    = "-1"
       cidr_blocks = ["0.0.0.0/0"]
     }
   }
   ```

   **Explanation:**

   - **Provider Block**: Specifies the AWS region.
   - **aws_instance Resource**: Defines an EC2 instance.
   - **aws_security_group Resource**: Creates a security group allowing SSH access.

2. **Initialize Terraform**

   Open a terminal and navigate to the directory containing `main.tf`. Run:

   ```sh
   terraform init
   ```

   ![Terraform Init](https://learn.hashicorp.com/tutorials/terraform/intro/images/terraform-init.png)

   *Source: [Terraform Tutorial](https://learn.hashicorp.com/terraform)*

3. **Plan the Deployment**

   Generate an execution plan:

   ```sh
   terraform plan
   ```

   Review the plan to confirm actions:

   ![Terraform Plan](https://learn.hashicorp.com/tutorials/terraform/intro/images/terraform-plan.png)

4. **Apply the Configuration**

   Apply the configuration to create the resources:

   ```sh
   terraform apply
   ```

   Confirm the action when prompted:

   ![Terraform Apply](https://learn.hashicorp.com/tutorials/terraform/intro/images/terraform-apply.png)

5. **Verify the EC2 Instance**

   Check the AWS Management Console to see your EC2 instance:

   ![AWS EC2 Console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/ec2-console-overview.png)

   *Source: [AWS EC2 Console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)*

6. **Destroy the Resources (Optional)**

   Remove resources when no longer needed:

   ```sh
   terraform destroy
   ```

   ![Terraform Destroy](https://learn.hashicorp.com/tutorials/terraform/intro/images/terraform-destroy.png)

## Summary

Using Terraform to manage AWS EC2 instances allows you to define and manage infrastructure as code, which simplifies deployment and management. Follow these steps to set up and manage your EC2 instances efficiently using both the AWS Management Console and Terraform.

For further information, refer to the [Terraform documentation](https://www.terraform.io/docs/providers/aws/index.html) and [AWS EC2 documentation](https://docs.aws.amazon.com/ec2/index.html).
