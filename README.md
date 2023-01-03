# 🚀 Getting Started

FL0 is a platform for automating the deployment Node.js apps. Just push code to your main branch and FL0 will build and deploy your app to a scalable, containerized infrastructure complete with Postgres database.

### Service Requirements

For your codebase to run in a FL0 service, it needs to meet the following requirements:

1. Compatible with Node v18.12.1
2. Contains a `package.json` file with a `start` command that runs a NodeJS server
3. If your project has a `build` command, the output folder must be called `dist`
4. Listens on a port specified by `process.env.PORT`&#x20;

### Git

{% hint style="info" %}
FL0 currently supports Github repositories, with more coming soon!
{% endhint %}

Each service must be connected to a single git repository. When creating a new Service, FL0 will prompt you to configure the connection to Github and select a repository.ar

* You must be an admin of your organization to install and configure the git connection
* Your FL0 organization can only be connected to a single Github organization
* Your Github organization must own the repositories being deployed on FL0

FL0 listens to commits made on the `main` branch of your repository.  Every commit will trigger a new deployment to the Dev environment.&#x20;

### Deployments: Without a Dockerfile

If your repository does **not** contain a Dockerfile in the root directory, FL0 will automatically create a container for you. In this case, deployments consist of three stages:

1. Setup: Prepares the deployment and connection to git
2. Build: Checks out the codebase, installs dependencies and creates a container
3. Deploy: Deploys the container to the FL0 environment

#### Package Managers

During the build step FL0 will automatically detect your chosen package manager by checking the following rules in this order:

1. If a `yarn.lock` file exists, use `yarn`
2. If a `pnpm-lock.yaml` file exists, use `pnpm`
3. If a `package-lock.json` file exists, use `npm`

The first detected package manager will be used, meaning if you have both `yarn.lock` and `package-lock.json`, `yarn` will take priority.

#### Deployment Steps

Deployments execute steps to install dependencies, build your codebase and run your app. Depending on the package manager, different steps will be run.

<details>

<summary>npm</summary>

1. Run `npm ci` to perform a clean install of dependencies
2. Run `npm run build` if it exists in `package.json`
3. Run `npm start`

</details>

<details>

<summary>yarn</summary>

1. Run `yarn install --immutable` to install dependencies
2. Run `yarn run build` if it exists in `package.json`
3. Run&#x20;

</details>

<details>

<summary>pnpm</summary>



</details>



### Deployments: With a Dockerfile

If your repository contains a Dockerfile in the root directory, FL0 will build a container using your provided Dockerfile.&#x20;

### Environment Variables

You can inject environment variables into your apps using the Environment Variables tab of your service. These values are available to Node under the `process.env` variable.

1. Variables cannot begin with `AWS` or include special characters
2. Values are limited to 4kb in size
3. Your app must be redeployed for the changes to take effect





