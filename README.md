# DevOps

## DevOps with AWS

Continuous Integration: Developer push code to repo. Build server builds and testing. \
Continuous Delivery: Automated deployment. Deployment server get the build and deploy to server. May involve manual steps to approve a deployment. \
Continuous Deployment: Full automation. Every code change is deployed all the way to production. No manual intervention. \

Source Repo: AWS Code Commit(private Git for AWS account) or GitHub(public) or bitbucket(private paid) \
Build Server: AWS CodeBuild or Jenkins CI Server \
Deployment Server: AWS CodeDeploy . For provisioning use AWS CloudFormation or AWS ElastcBeanStalk. \
AWS CodePipeLine:  To orchestrate everything. \

*AWS Code Commit*
Code stays in AWS cloud account. \
Create a repo under Code Commit Service under AWS account. \

*AWS CodeBuild*
Serverless, Docker based(use docker containers to build code) , Pay as you go. \
Build instructions will be specified in buildspec.yml file. \
Build logs are sent to S3 or Amazon CloudWatch Logs. \
Cloud Watch Metrics for build statistics. \
Create CodeBuild project under CodeBuild Service in your account. Configure Source as AWS CodeCommit or GitHub or S3. \
You can configure docker image for build server. (managed and custom options available). \
Buildspec.yaml: Configures phases: install, pre-build, build, post_build. Will contains commands to execute. We can set up variables and parameter-store, secret-manager. We can set up Artifacts. \ 
Envt Variables can be added to CodeBuild project also. Create secrets in System-Manager parameter. Attach SSM policy to code build role to access SSM. Using paramater store is secure way. \ 
Build outputs ( jar or image) as artifacts: Configure Artifact(S3) in CodeBuild project. Configure artifact location in buildspec.yaml. 

*AWS CodeDeploy*
Automate code deployment on EC2, ECS, Lambda and on premise. \
CodeDeploy agents run on instances where deployment should happen. As a DevOps person, we need to define - What is to be deployed, Where it should be deployed and how it should be deployed. This is defined in appspec.yml and configuration file. \
You have to provision all resources for CodeDeploy in advance. For deploying to an Ec2 instance, 1. create Ec2 instance. 2. Create IAM role with access to S3. 3. Login to Ec2 and install Code deploy Agent(by running sudo commands) . These script can be provided as user data script while creating Ec2 instance. 4. Add tags envt:prod 5. Create application in CodeDeploy. 6. Create Deployment group ( a set of Ec2 instances for application) 7. Create a role for code deploy. 8. Deployment type - Inplace and Blue/Green deployment 9. Choose tag group prod ( target all Ec2 instances with tag envt:prod) 10. Create deployment - Application Revision type - in Amazon S3. 11. Create a bucket and enable versioning 12. Place folder with appspec.yml file to s3. Code deploy with push this to desired folder in Ec2. \

Deployment type - Inplace: Instance will be offline for some time. Strategies - AllAtOnce, OnceAtATime, HalfAtATime or custom configuraion(80% at a time) . \
Deployment type - Blue/Green: Once new instances are up, then old instances removed. Mandatory to have load balancer and auto scaling group configured. \

appspec.yml : source, destination, hooks-> ApplicationStop, BeforeInstall, AfterInstall, ApplicationStart, ValidateService. Hooks have location parameter which points to scripts. These scripts are part of repo.  Execute scripts to stop server, start server(httpd), install dependencies. 

Rollback: U can configure in Deployment Group to rollback. If any deployment fails, then deployment is rollbacked( Rollback when a deployment fails). If not enabled, rollback should be manual. 

*AWS Code Pipeline*
Visual Workflow tool for Continuous Delivery. Configure Stages(build, test, deploy...) \
1. Create pipeline in Code Pipeline. \
2. Create new role. \
3. Choose S3 bucket for pipeline. \
4. Add Source Provider - AWS CodeCommit. Change Detection options - CloudWatch Event(recommended) or CodePipeline default(Pool for changes every 30 mins) \
5. Add deploy stage. Deploy Provider: CodeDeploy. Choose Deployment Group. Choose region. Supports multi regon deployment. \
6. Add manual approval steps for deploying changes to prod. 

## Azure DevOps

Azure Boards, Azure Repos, Azure Pipeline

Accessible through Azure DevOps Web Portal or Visual Studio 

Azure Pipeline: Pipeline contains stages. Stages contains tasks. 

## Teraform

Infrastrcuture Management Tool for managing Servers, Networking, Storage through code. Teraform is for provisioning. It is not a configuration management tool. Using teraform you provision a server. To turn it to a web server u need to use puppet. Teraform is used for Immutable infrastructure. Can work with Docker and K8s. 

1. Download Teraform CLI
2. Create a user in AWS account
3. Keep AWS credentials in a file

``
  provider "aws" {
    profile ="default"
    region  = "us-west-2"  
  }
  
  resource "aws-s3-bucket" "tf_cource" {
    bucket =" tf-course-2022"
    acl    ="private"
  }

``
terraform init \
terraform plan \
terraform apply \
terraform state \
terraform graph ( u can use WebGraphViz for seeing the graph) \

terraform plan -out "example.plan" \
terraform apply "example.plan" \

Terraform takes the code, compares it with state of resource and come up with an execution plan. Teraform use a *Resource Graph*(Directed Acyclic Graph) to come up with execution plan. It orders resource creation/modification based on execution plan.  \

State: State of your resources in AWS. State of local terraform files in local dir.(Saved in terraform.tfstate. Generated automatically) \

Best Practice : Teraform style. Documented in site. 

Teraform Modules

### Managing Terraform with Sentinel
Policy As Code - Policies are written using the Sentinel language.Policies are the guardrails that prevent Terraform runs from performing dangerous actions.\
Teraform Enterprise Cloud -> Create Organization -> Create Workspace -> Plan and Apply \
Rules are written in sentinel \
    configured_instance_type = tfplan.module().resources.aws_instance.web[0].applied.instance_type //Pull information from terraform plan. \
sentinel.hcl file - configure policies and enforcement level. soft-mandatory, hard mandatory.

Terraform Cloud: When we commit a Run is triggered. It will run Plan, Cost estimation, Policy Check and Apply pending. \
Foundational Policies library: Bench mark for each service provider. CIS policies. Copy Policy. Go to sentinel.hcl file and paste it there. Source parameter is referencing to  policy already implemented in foundational policy library.
OPA vs Sentinel: Goals are same. Using declarative policy engine to apply policy. Sentinel is created by HarshiCorp. OPA can be used with different products. \
Organization Level Policies in Teraform Enterprsie: Apply Sentinel Policy to all workspace within an organization .Add Policy Name. Add Policy Code - Add Sentinel code.
Policy sets are groups of policies that can be enforced on workspaces. \
A policy set is simply a directory structure containing a Sentinel configuration file and some policy files.\

    






