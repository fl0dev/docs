# Express

### Prerequisites

1. NodeJS + NPM installed
2. A FL0 account ([sign up here](https://app.fl0.dev/account/sign-up))
3. A Github account
4. Optionally clone the example repository [here](https://github.com/fl0zone/example-express)

### Set up the codebase

If you have an existing Express app, getting it up and running on FL0 is easy. Review the requirements below and make any necessary adjustments.

#### Listen to the right port

FL0 injects an environment variable called `PORT` into your application's container. Your Express app must listen on this port. Below is an example of an Express server configuration, with a fallback to port 3000 in the case of local development.

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

#### Include a start command

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

#### Configure your build command (optional)

If you compile your source code into another folder, make sure that:

1. You include a `build` script in `package.json`
2. Your build output folder is called `dist`

You may choose to do this if you are using Typescript, Babel, Webpack or any other framework that requires building/compiling. FL0 will detect if a `build` script is included and run `npm run build` while creating the container.

#### Host on Github

Right now FL0 supports Github, with Bitbucket and other platforms to be added soon. Make sure your code is hosted in a Github repository and you have permissions to install and configure third party Github apps.

### Create a database

In your FL0 project, create a new Database and take note of the connection details:

1. Hostname
2. Username
3. Password
4. Port
5. Database name

Use these details to connect to your database. When working locally, FL0 recommends using the [dotenv](https://www.npmjs.com/package/dotenv) package to load them from a `.env` file. The file might look something like below:

```
DATABASE_URL=postgres://{{username}}:{{password}}@{{{hostname}}:5432/{{databasename}}
```

###





