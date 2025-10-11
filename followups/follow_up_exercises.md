# Follow up Exercises

## Tasks

1. Explain how code on your delivery is packaged and deployed.
2. Explain what you understand of the approach to hosting apps in the course.
3. How does this differ from approaches you've used before? What's the impact of changing our mind? and the benefits?
4. Explain the troubleshooting steps you would use on a failing pipeline, either those you learned on this course or those you have used on your delivery.
5. Identify a problem or improvement you could do on a pipeline on your delivery. What did your team decide to do to fix or improve it (even if it hasn't been prioritised yet)?

## Stretch task
1. Terraform resources to build docker images and push them to ECR is not the only way, it's not even the most usual way using github actions! Try out a new pipeline with different github actions to build and deploy the ex 6 image to the ex 6 ECR. You could start from your ex 3 pipeline syntax to build and test the node code first.
2. If you identified a different approach to hosting apps containers, investigate more and explain your discovered new approach including, what are the pros and cons of adopting it, if our start point is where the course examples left off?


## Add your answers here, or in this folder

### 1. Explain how code on your delivery is packaged and deployed.

Our delivery uses AWS CodePipeline. The pipeline is split up into the following stages:

- Grab the repo from GitHub
- Runs tests
- Builds the frontend
- Deploys the frontend to the integration environment
- Runs e2e tests in the integration environment
- Deploys the frontend to the staging environment
- Requires a manual approval to deploy to production
- Deploys to production after a manual approval

### 2. Explain what you understand of the approach to hosting apps in the course.

Hosting apps is done through CI/CD which is short for Continuous Integration and Continuous Delivery. A CI/CD pipeline is an automated series of steps that take our source code from development to production. Examples of CI/CD services are GitHub Actions and CircleCI, but there are many others.

Different things can trigger a pipeline to execute e.g. git commits followed by pushes or merges to a branch. It can also be triggered on a schedule or executed manually e.g. through the Github Actions UI.

The CI part of the pipeline automates the process of integrating code changes. It checks that the code builds, tests and works properly before moving on to the CD part.

The CD part of the pipeline automates the process of getting the application available to users by deploying it to a chosen environment e.g. staging, testing, or production.

Pipelines are important for several reasons. Below are some examples

- It can check our tests pass on every push we make.
- It completes the series of steps consistently each time.
- Automation makes deploying less prone to human error.
- Automation saves time because developers are not required to complete the individual steps in the pipeline.

I also learnt on the course that using GitHub Actions to build pipelines may not be a good idea. The reason for this is because I noticed during one of the later exercises in a failing pipeline that my AWS account ID was being shown in the error logs on GitHub Actions. This can lead to security issues since our AWS account ID is supposed to be private. Since GitHub Actions is accessible to anyone if a repository is public, this means anyone would be able to see my AWS account ID from the error logs in GitHub Actions (e.g. on my delivery, our frontend repository is public, and anyone is able to access the Actions section of the repo including all error logs from failing pipelines). This is not an issue if your repo is private.

What is happening in the pipeline matters though. E.g. if your pipeline just runs tests, then you shouldn't expect any security issues when using GitHub Actions on a public repo (e.g. our pipeline on GitHub Actions just runs our tests so there's no security issues even though it's a public repo).

### 3. How does this differ from approaches you've used before? What's the impact of changing our mind? and the benefits?

Previously, I used Netlify to host and deploy a React app directly from a GitHub repository. I had a custom CI workflow defined in a ci.yaml file that ran on GitHub Actions to handle formatting, linting, type checking, testing, and building the app. Once the workflow passed successfully, Netlify automatically deployed the latest build from the main branch.

During the course, we used Terraform to automate building the environment that hosts the app. This provides more control and reliability than just deploying code with Netlify. Unlike Netlify, which only handles building and deploying the app, this approach automatically creates the infrastructure needed to run the app. This means the environment is reproducible, secure, and consistent, and makes it easy to scale or recreate the app in different environments.

### 4. Explain the troubleshooting steps you would use on a failing pipeline, either those you learned on this course or those you have used on your delivery.

When a pipeline fails, the first thing I would do is look at the logs in GitHub Actions, and see which step of the pipeline has failed. GitHub Actions can provide useful information as to why the pipeline has failed. For example, during the course, one of the pipelines in exercise 6 failed because the owner variable was set to "Adil-Rahman". The problem was that uppercase letters were not allowed so when I changed the owner variable to "adil-rahman", the pipeline passed. In this case, the logs provided a useful error message which helped me to resolve the pipeline and get it to pass. If the logs did not say the pipeline was failing due to the uppercase letters in the owner variable, I likely would not have been able to resolve this without getting help from Tess.

Sometimes, the logs may provide useful information as to why a pipeline is failing, but if you do not have sufficient domain knowledge, then you may end up stuck. For example, when the exercise 6 apprunner pipeline was failing, I got the below error message in GitHub Actions:

`Error: creating App Runner VPC Connector (adil-rahman-hello-svc): operation error AppRunner: CreateVpcConnector, https response error StatusCode: 400, RequestID: 21e19922-6b94-4640-bf5d-8f346ea4139a, InvalidRequestException: The subnet ids provided in requested are not in the same VPC. SubnetIDs: []
`

In this case, even though there was information as to why the pipeline failed, I was unable to resolve this failing pipeline because I do not have sufficient knowledge of AWS, VPCs and subnets.

From my delivery, I learned that a pipeline could fail not due to code changes made by yourself, but rather due to code changes made by another developer on the team. Recently, a team member accidentally had a duplicate line of code which was pushed and caused the tests and pipeline to fail. This was resolved quickly but it made me realise that if I see a pipeline failing after I push my code, that doesn't necessarily mean it was my code changes which caused the pipeline to fail. In these cases, the logs in GitHub Actions would clearly show which tests are failing so if it happens to be the case where tests that I haven't changed are failing, it could mean that someone else has broken the pipeline.

### 5. Identify a problem or improvement you could do on a pipeline on your delivery. What did your team decide to do to fix or improve it (even if it hasn't been prioritised yet)?

One improvement we can make is to switch from npm to pnpm as there have been some recent security issues with npm, and pnpm has stronger safeguards against certain dependency vulnerabilities through its strict package isolation model. This ensures that each package can only access the other dependencies it explicitly declares, rather than being able to access all packages in the project.

Also, pnpm is faster and more efficient, particularly during install and build steps. This would reduce build times in our pipeline and speed up deployments. The current time for the pipeline to finish running all steps is approximately 5 minutes, which is relatively short, so improving the speed isn't very important at this stage, but this could become longer as the project becomes bigger, so it might be useful later on.
