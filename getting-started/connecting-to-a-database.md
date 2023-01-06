# Connecting to a Database

### Introduction

Setting up a database in FL0 can be done in a couple of clicks! The steps on this page will show you how to add a Postgres database to your FL0 Project and connect it to a Service.

### Creating the database

1. From your FL0 Project's homepage, click the "Add new" button and select "New Postgres database" from the dropdown
2. That's it, there is no more steps!

Every database you create with FL0 comes with both Development and Production environments, with separate credentials for each.



### Connecting from a FL0 service

1. Navigate to the Environment Variables tab of your FL0 Service
2. FL0 will automatically detect the database and show a banner allowing you to import database credentials. Click the "Import database credentials" button
3. In your code, access the credentials using `process.env.DATABASE_URL`

How you use the `DATABASE_URL` environment variable will differ depending on your chosen database client.&#x20;

#### Sequelize

Sequelize has a number of ways to specify connection details. Below are a couple of examples.

Firstly, [directly specifying](https://sequelize.org/docs/v6/getting-started/#connecting-to-a-database) connection details:

```javascript
const sequelize = new Sequelize(process.env.DATABASE_URL)
```



Or via the use of a [config file](https://sequelize.org/docs/v6/other-topics/migrations/#configuration):

{% code title="src/config/config.json" %}
```json
{
  "local": {
    "use_env_variable": "DATABASE_URL",
    "logging": false
  },
  "development": {
    "use_env_variable": "DATABASE_URL",
    "logging": false
  },
  "production": {
    "use_env_variable": "DATABASE_URL",
    "logging": false
  }
}
```
{% endcode %}

{% code title="src/models/index.js" %}
```javascript
...
const env = process.env.NODE_ENV || 'development';

const config = require(__dirname + '/../config/config.json')[env];
const db = {};

let sequelize;
if (config.use_env_variable) {
  sequelize = new Sequelize(process.env[config.use_env_variable], config);
} else {
  sequelize = new Sequelize(config.database, config.username, config.password, config);
}
...
```
{% endcode %}

#### Prisma

{% code title="prisma/schema.prisma" %}
```javascript
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```
{% endcode %}

See Prisma's [documentation](https://www.prisma.io/docs/reference/database-reference/connection-urls) for more information.

### Connecting from an external client

FL0 databases can be accessed from a client like [PGAdmin](https://www.pgadmin.org/) from your own machine. The connection details can be found in FL0 on the "Connection Info" tab of the database.&#x20;
