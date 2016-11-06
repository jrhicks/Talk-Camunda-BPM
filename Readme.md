# Camunda Shared Engine and Process Application Deployment Lab

<img src="overview.png">

## Goal

1) Run a Tomcat server with a shared process engine and some general camunda apps that can be used for any process.

2) Setup a maven project that can build and deploy a camunda process.

3) Deploy the camunda process

## Run Tomcat Server

Docker is used to build a Tomcat server with system dependencies, general camunda web applications, and configurations useful for this lab.

```
cd tomcat-server
docker build . -t camunda
docker run -p 8080:8080 camunda
```

## Setup Maven Project (Eclipse Option)

1) Create project in Eclipse

2) Configure Builds: add tomcat7:deploy

3) Run build

## See Results

1) open http://localhost:8080/camunda

2) Create User

3) Start a Process
