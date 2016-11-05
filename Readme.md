# Shared, Container-Managed Process Engine Example

Learn to run and deploy process applications to Tomcat with a shared camunda process engine as depicted in the preferred Camunda architecture below:

<img src="shared-process-engine.png">

## Goals

[ x ] Install Camunda On Vanilla Tomcat using Shared Engine Configuration

[ x ] Configure Tomcat manager to accept deployments

[ x ] Install Rest API application with Basic HTTP Authentication

[ x ] Setup an example Spring Project for a camunda process for deployment to a Shared Engine Configuration running on Tomcat using the Maven deploy plugin

[ x ] Successfully deploy

[   ] Have the deployed application successfully start

  * 05-Nov-2016 14:36:57.745 SEVERE [http-nio-8080-exec-1] org.springframework.web.context.ContextLoader.initWebApplicationContext Context initialization failed
 org.springframework.beans.factory.CannotLoadBeanClassException: Cannot find class [org.camunda.bpm.engine.spring.application.SpringServletProcessApplication] for bean with name 'processApplication' defined in ServletContext resource [/WEB-INF/applicationContext.xml]; nested exception is java.lang.ClassNotFoundException: org.camunda.bpm.engine.spring.application.SpringServletProcessApplication

[   ] Access the process through the user TaskList web application

[   ] Access the process through the rest api


## Resources

* https://docs.camunda.org/manual/7.5/installation/full/tomcat/manual/ - Manual Install Camunda on Tomcat

* https://docs.camunda.org/get-started/spring/shared-process-engine/ - Setting up a process to deploy to a Tomcat w/ shared Process Engine

* https://docs.camunda.org/manual/7.5/user-guide/process-engine/process-engine-bootstrapping/#shared-container-managed-process-engine - Setting up a Tomcat w/ shared Process Engine


## Docker Folder

1) Docker folder contains a camunda folder where the instructions for Installing a shared camunda engine on a vanilla Tomcat were followed.

2) Conf folder has tomcat-users.xml which will be copied into place on docker build to enable Tomcat manager access

3) Dockerfile - Docker build

* cd docker

* docker build . -t camunda

* docker run -p 8080:8080 camunda

### Open Camunda Webapp to Be Prompted to create User

http://localhost:8080/camunda

### If things are working right the following should return some JSON

curl -X GET -H "Accept: application/hal+json" -H "Cache-Control: no-cache"  "http://admin:password@localhost:8080/engine-rest/task"

## loanapproval-spring

* A maven project for a loan application business process setup to deploy to Tomcat server.
