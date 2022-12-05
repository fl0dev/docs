# Services



### Service Requirements

For your codebase to run in a FL0 service, it needs to meet the following requirements:

1. Compatible with Node v18.12.1
2. Contains a `package.json` file with a `start` command that runs a NodeJS server
3. If your project has a `build` command, the output folder must be called `dist`
4. Listens on a port specified by `process.env.PORT`&#x20;

