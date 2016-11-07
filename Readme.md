# Camunda Shared Engine and Process Application Deployment Lab

<img src="images/overview.png">

## Goal

1) Run a Tomcat Server setup with [Camunda shared-engine architecture](https://docs.camunda.org/manual/7.4/introduction/architecture/#shared-container-managed-process-engine)

2) Verify Tomcat Server can accept Process Application Deployments

3) Build & deploy an existing process application with Eclipse IDE.

4) Create, build and deploy a new process application.

## Setup

This project uses the variable $LAB_HOME to refer to the folder where you've cloned this repo.

```
export $LAB_HOME=/users/jrhicks/labs
```

Checkout this repo

```
cd $LAB_HOME
git clone https://github.com/jrhicks/camunda-lab-1.git
```

This lab requires [docker engine installation](https://docs.docker.com/engine/installation/).


## Run a Tomcat Server setup with Camunda shared-engine architecture

Docker is used to build a Tomcat server with system dependencies, some general camunda web applications, and configurations useful for this lab.

```
cd tomcat-server
docker build . -t camunda
docker run -p 8080:8080 camunda
```

## Verify Tomcat Server can accept Process Application Deployments (Using Docker)

Typically we don't use docker to build and deploy a process application because we can directly deploy from our IDE.  However, since the docker method is very reproducible it serves as a good mechanism to verify the Tomcat Server is setup correctly to accept deployments.

```
cd process-maven-projects/monthly-meetup

docker run -it --rm \
       -v "$(pwd)":/opt/maven \
       -w /opt/maven \
       --net="host" \
       maven:3.2-jdk-8 \
       mvn -s settings.xml clean tomcat7:deploy
```

Confirm Deployment by opening [http://localhost:8080](http://localhost:8080), create user, login, start process.

## Create, Build and Deploy a new process application

Ultimately we want to create our own process applications to deploy.  First we setup our Eclipse IDE and verify correct setup by deploying an existing process application.

* Install [Eclipse IDE For Java Developers - Neon1a](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neon1a)

* Install [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

* Import project into eclipse

 * Open Eclipse

 * File Menu -> Import -> Maven -> Existing Maven Projects -> Next

* Create run configuration to deploy to Tomcat Server

  * Run Menu -> Run Configurations:

<img src="/images/run_configurations_1.png" align="left" width="200px">

    * Right Click On Maven and select New

  * Configure a deploy build job

<img src="/images/run_configurations_2.png" align="left" width="200px">

    1) Name the run Configuration

    2) Click Filesystem and select folder $LABS_HOME/camunda_lab_1/process-maven-projects/monthly-meetup

    3) Set Goals to: tomcat7:deploy

    4) Note the path of user settings.  In later steps we will refer to this as $USER_SETTINGS_PATH

    5) Apply

    6) Close

  * Copy user_settings.xml into $USER_SETTINGS_PATH

   * cp $LABS_HOME/camunda_lab_1/process-maven-projects/monthly-meetup/settings.xml $USER_SETTINGS_PATH

  * Configure a redeploy build job.

   * Create another run configuration.  Follow the same steps as build deploy job except name it redeploy and set the goal to: tomcat7:redeploy

  * Run Build

<img src="/images/run_configurations_3.png" align="left" width="200px">

   * Depending on situation either deploy or redeploy.

## Create, Build and Deploy a new process application

<img src="images/overview2.png">

Camunda provides [Maven Project Templates (Archetypes)](https://docs.camunda.org/manual/7.4/user-guide/process-applications/maven-archetypes/) to enable a quick start to developing process applications.

Follow these [detailed instructions](https://docs.camunda.org/manual/7.4/user-guide/process-applications/maven-archetypes/#detailed-instructions) to register camunda's architype repository with eclipse or
