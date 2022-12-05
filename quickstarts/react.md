---
description: How to configure a React app to run on FL0
---

# React

{% hint style="info" %}

{% endhint %}

You can build and deploy a React application on FL0 with just a few easy steps:

1. Create an Express server to send the React static files to the browser
2. Change the name of the `build` folder to `dist`
3. Modify the `start` command in `package.json`&#x20;

### Why are these changes required?

TLDR; FL0 currently supports **backends** on NodeJS but React is a **frontend** framework.

React is a frontend framework, meaning it gets built and converted to static files, like HTML, CSS and Javascript. Currently the services you create on FL0 are for running NodeJS servers. We are working on supporting frontend projects too. For now, you can easily serve static files with a basic Express server.&#x20;

### Creating the server

1. Install `express` by running `npm install express`
2. Create a new folder in the root of your React project called `server`
3. Create a new file in the `server` folder called `index.js`

{% code title="server/index.js" %}
```javascript
const express = require("express");
const path = require("path");

const app = express(); // create express app

// add middleware
app.use(express.static(path.join(__dirname, '..', 'dist')));

// start express server on port 5000
const port = process.env.PORT ?? 3000;
app.listen(port, () => {
  console.log("server started on port " + port);
});
```
{% endcode %}

### Change the name of the build folder

Open up `package.json` and modify the `build` script to look like the following:

{% code title="package.json" %}
```json
{
  ...
  "scripts": {
    ...
    "build": "react-scripts build && mv build dist",
    ...
  },
  ...
}
```
{% endcode %}

This will run the normal build, and then rename the `build` to `dist`.

### Modify the start command

By default, the React `start` command boots up a dev server and watches for changes. We want to change this to use our new Express server.&#x20;

1. Rename the `start` command to `start:client`
2. Add a new `start` command that will kick kick off the Express server

{% code title="package.json" %}
```json
{
  ...
  "scripts": {
    ...
    "start": "node ./server/index.js",
    "start:client": "react-scripts start",
    ...
  },
  ...
}
```
{% endcode %}

### Wrapping up

That's it! Commit your changes to the `main` branch to trigger a new build. Your React app should be available at the root URL of your service.
