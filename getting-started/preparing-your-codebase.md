# Preparing Your Codebase

### Introduction

Getting an app up and running on FL0 is easy! Whether you have an existing repository or are starting from scratch, FL0 lets you automatically deploy to a scalable, containerized infrastructure with every commit.

Follow the steps in this guide to get your app deployed on FL0.

### Prerequisites

1. A FL0 account
2. Your code stored in a Github repository

### Listen to the right port

FL0 injects an environment variable called `PORT` into your application's container. Your  app must listen on this port. Below are some examples in common programming languages:

{% tabs %}
{% tab title="Express.js" %}
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
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"errors"
	"fmt"
	"io"
	"net/http"
	"os"
)

func getRoot(w http.ResponseWriter, r *http.Request) {
	fmt.Printf("got / request\n")
	io.WriteString(w, "This is my website!\n")
}
func getHello(w http.ResponseWriter, r *http.Request) {
	fmt.Printf("got /hello request\n")
	io.WriteString(w, "Hello, HTTP!\n")
}


func main() {
	http.HandleFunc("/", getRoot)
	http.HandleFunc("/hello", getHello)
	
	port, exists := os.LookupEnv("PORT")
	if !exists {
		port = "80"
	}

	err := http.ListenAndServe(":" + port, nil)
}
```
{% endtab %}

{% tab title="Python" %}
{% code title="app.py" %}
```python
import os
from flask import Flask

app = Flask(__name__)

port = int(os.environ.get("PORT", 5000))

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=port)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Using Built-in Language Support

FL0 has out-of-the-box support for many languages, which means your code will be built and deployed automatically without any special configuration required. Below is a list of supported languages:

1. Java
2. Node.js
3. .NET Core
4. Go
5. Python
6. PHP
7. Ruby

For more complex scenarios where specific **build**, **test** or **lint** steps are required, see the section below on using a **Dockerfile**.



### Using a Dockerfile

When building your project, FL0 runs the `docker build` command and requires a Dockerfile to be present in the root of your repository. Your Dockerfile should be configured to build and start your application. Below are some examples of multi-stage Dockerfiles with the following stages:

1. `development` stage works on a local machine and watches for file changes, reloading as necessary
2. `builder` stage is used to build a production version of your app
3. `production` stage is what finally gets deployed to FL0. It is designed to only contain production dependencies and be as small as possible. Assets from the `builder` stage are copied into the `production` stage

To read more, see our blog post on creating [The perfect multi-stage Dockerfile for Node.js apps](https://blog.fl0.com/the-perfect-multi-stage-dockerfile-for-node-js-apps-981dd61bdb34).

{% hint style="info" %}
As long as you have a Dockerfile, FL0 can build Rust, Go, Python or any other language!
{% endhint %}

{% tabs %}
{% tab title="Javascript" %}
```docker
FROM node:16 as development
WORKDIR /usr/src/app
COPY package*.json tsconfig.json ./
RUN npm install
COPY ./src ./src
EXPOSE 80
CMD [ "npm", "run", "start:dev" ]

FROM development as builder
WORKDIR /usr/src/app
RUN rm -rf node_modules
RUN npm ci --only=production
EXPOSE 80
RUN npm run build
CMD [ "npm", "start" ]
 
FROM alpine:latest as production
RUN apk --no-cache add nodejs ca-certificates
WORKDIR /root/
COPY --from=builder /usr/src/app ./
CMD [ "node", "./build/index.js" ]
```
{% endtab %}

{% tab title="Typescript" %}
```docker
FROM node:16 as development
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY tsconfig.json tsconfig.build.json ./
COPY ./src ./src
EXPOSE 80
CMD [ "npm", "run", "start:dev" ]

FROM development as builder
WORKDIR /usr/src/app
RUN rm -rf node_modules
RUN npm ci --only=production
RUN npm i -g @nestjs/cli @types/node
RUN npm run build
EXPOSE 80
CMD [ "npm", "start" ]

FROM alpine:latest as production
RUN apk --no-cache add nodejs ca-certificates
WORKDIR /root/
COPY --from=builder /usr/src/app ./
CMD [ "node", "dist/main" ]
```
{% endtab %}

{% tab title="Go" %}
```docker
FROM golang:alpine AS development
ENV GO111MODULE=on \
    CGO_ENABLED=0
WORKDIR /app
COPY . .
RUN go mod download
RUN go get github.com/githubnemo/CompileDaemon
RUN go install github.com/githubnemo/CompileDaemon
CMD CompileDaemon -log-prefix=false -build="go build -o main" -command="./main"

FROM golang:alpine AS builder
ENV GO111MODULE=on \
    CGO_ENABLED=0
WORKDIR /app
COPY . .
RUN go mod download
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest as production
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=production /app/main .
CMD ["./main"]
```
{% endtab %}

{% tab title="Python" %}
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages
RUN pip install --trusted-host pypi.python.org Flask

# Run the command to start the Flask app
CMD ["python", "app.py"]

```
{% endtab %}
{% endtabs %}



###
