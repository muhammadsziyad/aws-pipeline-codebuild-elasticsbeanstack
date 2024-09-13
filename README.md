
# AWS CI/CD Pipeline with CodePipeline, CodeBuild, and Elastic Beanstalk

This project demonstrates how to build a basic Continuous Integration and Continuous Deployment (CI/CD) pipeline using AWS CodePipeline and CodeBuild. The pipeline will automatically trigger when code changes are pushed to a GitHub repository, build the code, and deploy it to an Elastic Beanstalk environment.

## Project Overview
- **Objective**: Automate the process of building, testing, and deploying an application using AWS CodePipeline, AWS CodeBuild, and Elastic Beanstalk.
- **Services Used**: AWS CodePipeline, AWS CodeBuild, AWS CodeCommit (or GitHub), AWS Elastic Beanstalk, AWS IAM.
- **Outcome**: A fully automated pipeline that will pull code from a GitHub repository, build the application, and deploy it to an Elastic Beanstalk environment.

## Requirements
1. **AWS Account**: Ensure you have an active AWS account.
2. **GitHub Repository**: Create or have a GitHub repository with a sample web application. You can use a simple Node.js, Python, or PHP application.
3. **AWS Elastic Beanstalk Environment**: Set up an Elastic Beanstalk environment that will host the web application.
4. **IAM Role**: Ensure you have permissions to create IAM roles, CodePipeline, and CodeBuild.
5. **AWS CLI** (optional but recommended): For managing resources via the command line.

---

## Steps to Set Up CI/CD Pipeline

### Step 1: Create a GitHub Repository
1. **Create a GitHub Repository**:
   - If you don’t have one already, create a new repository on GitHub with a sample application (e.g., a simple Node.js app).
   - Push the source code to this repository.

### Step 2: Set Up Elastic Beanstalk Environment
1. **Create an Elastic Beanstalk Application**:
   - Go to the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk/home).
   - Click **Create Application**, give it a name (e.g., `MyApp`), and choose a platform (Node.js, Python, etc.).
   - Configure the environment as per your needs, such as instance type and auto-scaling settings.
   - Click **Create Environment**.

2. **Note Down Environment Details**:
   - Once the environment is created, note the environment URL and name. This will be used to deploy the application later.

### Step 3: Create the CI/CD Pipeline with CodePipeline

1. **Go to CodePipeline**:
   - Navigate to the [AWS CodePipeline console](https://console.aws.amazon.com/codepipeline/home).
   - Click **Create Pipeline**.

2. **Pipeline Settings**:
   - Give your pipeline a name (e.g., `MyAppPipeline`).
   - Create a new service role or use an existing role to allow CodePipeline access to other services.
   - Click **Next**.

3. **Source Stage**:
   - **Source provider**: Choose **GitHub**.
   - **Connect to GitHub**: You'll need to authenticate with your GitHub account.
   - **Repository**: Select your GitHub repository.
   - **Branch**: Choose the branch you want to trigger the pipeline (e.g., `main` or `master`).
   - Click **Next**.

4. **Build Stage (CodeBuild)**:
   - **Build provider**: Choose **AWS CodeBuild**.
   - **Create a new build project** (or select an existing one):
     - Give the project a name (e.g., `MyAppBuild`).
     - Choose a runtime environment for your app (e.g., Node.js for Node.js applications).
     - Select an appropriate build specification:
       - Either use the default build settings or define your custom build commands using a `buildspec.yml` file.
     - Example `buildspec.yml` for a Node.js application:
       ```yaml
       version: 0.2
       phases:
         install:
           commands:
             - npm install
         build:
           commands:
             - npm run build
       artifacts:
         files:
           - '**/*'
       ```
     - **Environment**: Choose the build environment (e.g., `Ubuntu` image).
     - Set the compute type to `Build.general1.small` (or larger for more intensive builds).
   - Click **Next**.

5. **Deploy Stage (Elastic Beanstalk)**:
   - **Deploy provider**: Choose **AWS Elastic Beanstalk**.
   - **Application name**: Select the Elastic Beanstalk application you created earlier.
   - **Environment name**: Select the environment where you want to deploy your application.
   - Click **Next**.

6. **Review and Create Pipeline**:
   - Review the pipeline stages: Source, Build, and Deploy.
   - Click **Create Pipeline**.

### Step 4: Configure CodeBuild to Access Elastic Beanstalk

1. **IAM Role for CodeBuild**:
   - Go to the [IAM Console](https://console.aws.amazon.com/iam/home) and find the IAM role that was created for CodeBuild.
   - Attach the **`AWSElasticBeanstalkFullAccess`** policy to this role so that CodeBuild can deploy the application to Elastic Beanstalk.

### Step 5: Test the Pipeline

1. **Trigger the Pipeline**:
   - Go to your GitHub repository and push some code changes (e.g., modify a file or add a new feature).
   - This should automatically trigger the pipeline. You can monitor the progress in the **CodePipeline console**.

2. **Monitor the Build and Deployment**:
   - In the CodePipeline console, you’ll see the progress of each stage (Source, Build, and Deploy).
   - If the build succeeds, the new version of the app will be deployed to your Elastic Beanstalk environment.

3. **Verify the Deployment**:
   - Once the deployment is successful, go to your Elastic Beanstalk environment URL (e.g., `myapp-env.eba-xxxxxxxx.us-west-2.elasticbeanstalk.com`).
   - Verify that the new version of your application is live.

### Step 6: Clean Up Resources

- After you’re done, you can delete the resources to avoid incurring charges:
  - Delete the Elastic Beanstalk environment.
  - Delete the CodePipeline and CodeBuild project.
  - Remove any additional resources like S3 buckets or IAM roles created during the process.

---

## Additional Enhancements (Optional)
- **Add Unit Tests**: Modify the `buildspec.yml` to include unit tests for your application before deploying it.
- **Add Approval Stage**: Add a manual approval stage to review the deployment before it proceeds to production.
- **Configure Notifications**: Set up SNS notifications to get alerts when the pipeline fails or succeeds.

## Learning Outcomes
- Understand the fundamentals of CI/CD and its importance in modern software development.
- Gain hands-on experience with AWS services such as CodePipeline, CodeBuild, and Elastic Beanstalk.
- Learn how to integrate external code repositories (e.g., GitHub) with AWS services for automatic deployments.

This project is an excellent starting point for learning about DevOps practices on AWS, and it can be extended to more complex environments as you advance.
