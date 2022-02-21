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


## DevOps with Azure


