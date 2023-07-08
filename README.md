
# AWS Continuous Integration (ci) Project Using Node.js App

## Prerequisites:

- An AWS account
- A Node.js application repository hosted on a version control system like GitHub or AWS CodeCommit

## Step 1: Preparing the Source Code:

- Make a directory named **aws-cicd** and CD into it using the command:
> `mkdir aws-cicd && cd $_`
- create an **index.js** file that will contain the javascript code
- Also create an **index.html** file that'll containt the HTML content
- Create a **buildspec.yml** file which contains instruction for the pipeline
- Create a **Dockerfile** that contains the instructions to build docker image
- The above files were created using 
> ` code index.js index.html buildspec.yml Dockerfile`
- Installs both nodejs and NPM:
> ``sudo apt install nodejs -y`` 
- NPM is a package manager for Node and it is used to install Node modules & packages and to manage dependency conflicts.
- The  contents of the directory as showwn in the below diagram:

> ![files](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/8dc8562f-72dd-4099-81b1-15de76960860)

- Initialized the directory for git, and the subsequent command to commit and push the directory contents to the repository using
> `git init`
> `git add .`
> `git commit`
> `git push`

---
## Step 2: Set up AWS CodeCommit Repository:`

- On the AWS console, open the CodeCommit service
- Navigate to the "Repositories" and click on "Create Repository"
- Give it a desired name and description. In my case, I name my repo Trios-2 
- Click on create to create the repo and you'll be returned to the repository page with your created repo as shown below image:

> ![codecommit-repo](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/311957b9-b50e-4bde-a9a4-29a0883446d3)

- After pushing, the contents of the repo should look ike this:

![repo-contents](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/a99e4c05-4086-4c79-b107-af7683c322a3)

---

## Step 3: Set up an AWS CodeBuild Project:
- On the AWS Management Console, open the CodeBuild service.
- Click on "Create build project" and provide a name and description for the project.

- Under "Source provider," select the version control system where your Node.js application repository is hosted and in this case, CodeCommit.
- Connect to your repository and select the appropriate branch (Master in my case) as shown in the accompany diagram.

>![creating-build-contd](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/161b418b-eee9-433e-9465-d6a18340004a)


>### Configure the build settings:
>- Environment: Choose the environment image that suits your application. Although, no variable specify for in this project.
>- Buildspec: Create a buildspec.yml file in your repository that defines the build steps and environment.
>- Artifacts: Configure the output location for build artifacts.
>- Adjust any additional settings, such as VPC configuration or build timeouts. In my own case, There's no need configuring this.
>- Save the project configuration.

>![build-project](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/00cd5739-a411-47e1-8bcd-71763007ab6e)

---

## Step 3: Setting up the CodePipeline:

- Go to the AWS Management Console and search for "CodePipeline".
- Click on "Create pipeline" and follow these steps:
- Provide a pipeline name.
- For "Source provider," select "AWS CodeCommit".
- Select the repository and branch you want to use.
- For "Build provider," choose "AWS CodeBuild".
- Configure the build provider with the appropriate settings (e.g., operating system, runtime).
- For "Deployment provider," it is skipped for now as it will be done in the subsequent project 
- Attach **_AmazonEC2ContainerRegistryFullAccess_** as a permissions policy to the pipeline. This will grant the pipeline permission to the container registry

>![policy](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/76668b99-433c-4063-a7a1-e717203670ad)

- AWS CodeBuild will automatically pull the latest code changes, execute the build steps defined in the buildspec.yml file, and produce build artifacts.
- The pipeline initially print an error messages below, and subsequently got interrupted. 

![pipeline-error](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/e062ee87-73cf-4e92-8754-28c6c9d05efa)

- This was troubleshooted and resolved after several attempts as shown in the following images

![build-hx-status](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/07e49d1e-e346-4320-8f3a-c4e9e2e5f85b)

![build-success](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/83cb9add-559e-46ce-91b3-736b59c4eee3)

![ci-pipeline](https://github.com/Y2O-Dev/aws-cicd/assets/114786664/bfe0ffa4-69e5-4b96-8873-db0cad696ac3)

---
## END
