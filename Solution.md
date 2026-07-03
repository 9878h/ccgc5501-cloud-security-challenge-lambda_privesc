# Lab 3 – IAM Privilege Escalation by Lambda Function Invocation
Objective
The objective of this lab was to deploy a vulnerable AWS environment using Terraform, configure AWS CLI with the provided IAM user credentials, enumerate IAM resources, and investigate how a low-privileged IAM user could gain additional permissions through IAM role assumption. The lab demonstrates how improper IAM configuration can lead to privilege escalation and highlights AWS security best practices.

Rerequisites

- AWS Account
- Terraform
- AWS CLI
- Git
- VS Code


Step-by-Step Solution

Step 1 – Fork Repository:Creating a personal copy of the lab repository for completing the assignment.
I forked the GitHub repository to my own GitHub account.

Repository:
https://github.com/humbercloudsecurity/ccgc5501-cloud-security-challenge-lambda_privesc


Step 2 – Clone Repository:
Download the lab files to my local computer.
Command: git clone https://github.com/9878h/ccgc5501-cloud-security-challenge-lambda_privesc.git
Command: cd ccgc5501-cloud-security-challenge-lambda_privesc
Step 3 – Verify Repository Files
I verified the repository contained the required Terraform files.

Files: These files define the vulnerable AWS infrastructure.
- README.md
- iam.tf
- provider.tf
- variables.tf
- outputs.tf
  
Step 3 – Initialize Terraform:Downloaded the AWS provider plugins and initialized the Terraform working directory.

Command: terraform init
Result:
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

Step 4 – Deploy Lab: Deploy the AWS resources required for the privilege escalation scenario.
Command: terraform apply
Result: Apply complete! Resources: 9 added, 0 changed, 0 destroyed.

Outputs:

aws_account_id = "056449378674"
chris_access_key_id = "*********"
chris_secret_access_key = <sensitive>
chris_username = "chris-lambda_privesc"
debug_role_arn = "arn:aws:iam::056449378674:role/debug-role-lambda_privesc"
lambda_manager_role_arn = "arn:aws:iam::056449378674:role/lambdaManager-role-lambda_privesc"
scenario_goal = "Acquire full admin privileges starting as the low-privileged 'chris' user"
scenario_name = "lambda-privesc"
start_instructions = <<EOT

Step 5 – Obtain Chris Credentials: Retrieve the AWS Access Key ID and Secret Access Key for the low-privileged IAM user Chris.

Commands: terraform output -raw chris_secret_access_key
Command: terraform output -raw chris_secret_access_key

Step 6 – Configure AWS CLI:Configure the AWS CLI to use Chris's credentials.
Commands: aws configure --profile chris
AWS Access Key ID [None]:*********
AWS Secret Access Key [None]: *********
Default region name [None]: us-east-1
Default output format [None]: json

Step 7 – Verify Identity: Verify that the AWS CLI is using the correct IAM user.

Commands: aws sts get-caller-identity --profile chris

Step 8 – Enumerate IAM: List all IAM users available in the AWS account.

Commands:aws iam list-users --profile chris


Step 9 – Assume Lambda Manager Role: List all IAM users available in the AWS account.

Commands:aws iam list-roles --profile chris
Step 10- Verify AdministratorAccess:Verify that the AdministratorAccess policy was successfully attached to the Chris IAM user.

Command:aws iam list-attached-user-policies --user-name chris-lambda_privesc
Result:
The output displayed the AdministratorAccess policy attached to the user, confirming that the lab objective was achieved.

Reflection:
This lab improved my understanding of AWS Identity and Access Management (IAM), Terraform, and AWS CLI. I learned how attackers may identify misconfigured IAM permissions and how role assumption can provide additional privileges when trust relationships are not carefully designed. The lab demonstrated why the Principle of Least Privilege is important and why organizations should carefully review IAM roles, trust policies, and permissions before deployment.

Security Recommendations:
- Follow the Principle of Least Privilege.
- Avoid assigning AdministratorAccess to service roles.
- Restrict iam:PassRole permissions.
- Review IAM trust relationships regularly.
- Monitor AssumeRole activity using AWS CloudTrail.
- Audit IAM users, roles, and policies periodically.

Conclusion:
This lab demonstrated how IAM misconfigurations can introduce security risks in AWS environments. Through Terraform deployment, AWS CLI configuration, IAM enumeration, and role assumption, I gained practical experience with AWS IAM security concepts and learned how organizations can better secure cloud environments.
