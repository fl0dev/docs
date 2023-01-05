# Deploying To Development

### Introduction

Services deployed on FL0 automatically have a Development and Production environment. When you make a commit to your `main` branch, FL0 will automatically run a build process to deploy your latest changes to the Development environment. You can then manually deploy from Development to Production.

The build process performs the following tasks:

1. Checks out your codebase from Git
2. Runs `npm install` to install dependencies
3. Runs `npm run build` (if found in `package.json`) to build your project
4. Creates a container image of your app and deploys it to Development
5. Runs `npm start` to boot your app

### Trigger a build

Builds can be triggered manually or by pushing a commit to the repository.&#x20;

#### Automatic builds

1. Make a change inside your codebase
2. Commit your change and push it to the `main` branch
3. The change will automatically be detected by FL0 and built

#### Manual builds

1. Copy the [SHA](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History) of a commit from your repository
2. Navigate to the Deployment tab of the FL0 Service you wish to deploy
3. Click the "Deploy manually" button and paste in the SHA
4. The chosen commit will be built and deployed by FL0



