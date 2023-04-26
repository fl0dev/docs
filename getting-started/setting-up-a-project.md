# Setting Up a Project

### Introduction

A project in FL0 represents a collection of related apps and databases. In FL0, apps are containers created from your code. Databases can be created and deployed separately to apps.

{% hint style="info" %}
Currently FL0 supports the Postgres database type. Additional databases are coming soon.
{% endhint %}

### Prerequisites

1. Admin permissions for your Github organization

### Creating a Project

1. From inside your FL0 Workspace, click the **New project** button and give it a name
2. Give your project a name and save it

### Add an App

1. From your newly created Project, click the **Add new** button and select **Deploy your app** from options shown
2. Follow the prompts to [Connect your Github Account](../platform/workspaces.md#connecting-a-workspace-to-github) to FL0. You will need admin permissions to install the FL0 Github App
3. Click the **Connect** button on the repository you wish to deploy
4. Give your App a name. This name will form part of the URL
5. Choose the **Development** environment for deployment
6. Set up [Manual or Automatic Deployments](../platform/apps.md#automatic-vs-manual-deployments)
7. Add any required [Environment Variables](../platform/apps.md#environment-variables)

Once connected, FL0 will start building your App from the latest commit to the specified branch (automatic mode) or wait for a manual deployment to be made (manual mode).

