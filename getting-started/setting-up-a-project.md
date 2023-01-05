# Setting Up a Project

### Introduction

A project in FL0 represents a collection of related services and resources. In FL0, services are the apps you deploy from code. Resources are supporting systems like databases, storage accounts, message queues and more.&#x20;

{% hint style="info" %}
Currently FL0 supports single services within a Project. Multiple services will be possible in the near future.
{% endhint %}

{% hint style="info" %}
Currently FL0 supports the Postgres database resource type. Additional resources are coming soon.
{% endhint %}

### Prerequisites

1. Admin permissions for your Github organization

### Create a project

1. From inside your FL0 organization, click the "New Project" button&#x20;
2. Give your project a name. This name will form part of your service URL

Once a project has been added to your organization, it will have a URL in the format:

```
https://{organization}.api.fl0.dev/{project}/{service}
```

### Add a service

1. From your newly created Project, click the "Add new" button and select "Service" from the dropdown list
2. Follow the prompts to connect your Github account to FL0. You will need admin permissions to install the FL0 Github App
3. Click the "Connect" button on the repository you wish to deploy
4. Give your Service a name. This name will form part of the URL

Once connected, FL0 will start building your Service from the latest commit to the `main` branch.&#x20;

