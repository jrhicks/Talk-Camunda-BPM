# Shared, Container-Managed Process Engine Example

Learn to run and deploy process applications to Tomcat with a shared camunda process engine as depicted in the preferred Camunda architecture below:

<img src="shared-process-engine.png">

## Goals

[ x ] Install Camunda On Vanilla Tomcat using Shared Engine Configuration

[ x ] Configure Tomcat manager to accept deployments

[ x ] Install Rest API application with Basic HTTP Authentication

[ x ] Setup an example Spring Project for a camunda process for deployment to a Shared Engine Configuration running on Tomcat using the Maven deploy plugin

[ x ] Successfully deploy

[ X ] Have the deployed application successfully start

[ X ] Access the process through the user TaskList web application

[ X ] Access the process through the rest api


## Resources

"Manual installation of camunda on vanilla Tomcat"(https://docs.camunda.org/manual/7.5/installation/full/tomcat/manual/)

* Tomcat w/ Shared Process Engine

* Rest API with Basic Authentication

* Tomcat User For Maven Plugin Deployments

* TaskList, Cockpit, & Admin

"Maven Archetypes for Process Application (Servlet WAR)"(https://docs.camunda.org/manual/7.4/user-guide/process-applications/maven-archetypes/)

* Register Maven Registry with Eclipse

* Create project setup to Build, Test & Deploy

* BPM Process Application designed to run on Tomcat w/ Shared Process Engine

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

## Random Resources of Various Value

* https://docs.camunda.org/get-started/spring/shared-process-engine/ - Setting up a process to deploy to a Tomcat w/ shared Process Engine

* https://docs.camunda.org/manual/7.5/user-guide/process-engine/process-engine-bootstrapping/#shared-container-managed-process-engine - Setting up a Tomcat w/ shared Process Engine

* https://groups.google.com/d/msg/camunda-bpm-users/X5eV98ok0LY/_MqUhZ2MzO0J - Troubleshooting Tips

 * https://github.com/camunda/camunda-docs-manual/blob/master/content/user-guide/spring-framework-integration/configuration.md#configure-a-container-managed-process-engine-as-a-spring-bean - Some similar language found here as found in the Troubleshooting Tips
