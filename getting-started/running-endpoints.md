# Running Endpoints

### Introduction

Once your codebase is deployed on FL0, you can access the endpoints it exposes using the automatically generated URL.

### Finding the URL

1. Navigate to your FL0 Service
2. Switch to the "Overview" tab
3. Copy the URL

FL0 URLs are in the  following format for Development:

```
https://{organization}.api-dev.fl0.dev/{project}/{service}/{route}
```

And Production:

```
https://{organization}.api.fl0.dev/{project}/{service}/{route}
```

### Run an endpoint

Take the URL from the previous step and paste it into your browser, Postman or your frontend codebase to start consuming the endpoints exposed by your FL0 service.

### Reading logs

FL0 captures ouptut from your services and makes it available in realtime on the Logs tab.

1. Add a `console.log("My message")` statement to an endpoint in your codebase
2. Commit your changes and push them to the `main` branch
3. Open the Logs tab in FL0
4. Run the endpoint and observe the logs appearing in realtime
