# Preparing Your Codebase

### Introduction

Getting a NodeJS app up and running on FL0 is easy! Whether you have an existing repository or are starting from scratch, FL0 lets you automatically deploy to a scalable, containerized infrastructure with every commit to your `main` branch.

Follow the steps in this guide to get your app deployed on FL0.

### Prerequisites

1. A FL0 account (request alpha access [here](https://fl0.com))
2. A Github account
3. A NodeJS repository

### Listen to the right port

FL0 injects an environment variable called `PORT` into your application's container. Your  app must listen on this port. Below is an example of an Express server configuration, with a fallback to port 3000 in the case of local development.

{% code title="app.js" %}
```javascript
...
const port = process.env.PORT ?? 3000;

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
...
```
{% endcode %}

### Include a start command

When building your project, FL0 runs the `npm start` command to boot your app. This means you need to have a [start script](https://docs.npmjs.com/cli/v9/using-npm/scripts#npm-start) inside your `package.json`. If you include the [main](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#main) field, FL0 will use that instead of running `npm start`.

```
{
  ...
  "main": "src/app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node ./src/app.js"
  },
  ...
}

```

### Configure your build command (optional)

If you compile your source code into another folder, make sure that:

1. You include a `build` script in `package.json`
2. Your build output folder is called `dist`

You may choose to do this if you are using Typescript, Babel, Webpack or any other framework that requires building/compiling. FL0 will detect if a `build` script is included and run `npm run build` while creating the container.

###
