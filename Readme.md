# Camunda Shared Engine and Process Application Deployment Lab

<img src="images/overview.png">

## Goal

1) Run a Tomcat server which is setup for the [Camunda shared-engine architecture](https://docs.camunda.org/manual/7.4/introduction/architecture/#shared-container-managed-process-engine)

2) Build and deploy a process application from the [Maven Archetypes for Process Application (Servlet WAR)](https://docs.camunda.org/manual/7.4/user-guide/process-applications/maven-archetypes/)

3) Setup a development environment in Eclipse for building and deploying proces applications.

4) Create

## Get Lab Code

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


## Run Tomcat Server

Docker is used to build a Tomcat server with system dependencies, general camunda web applications, and configurations useful for this lab.

```
cd tomcat-server
docker build . -t camunda
docker run -p 8080:8080 camunda
```

## Build & Deploy Process Application (Docker Option)

Java's build tool Maven is used to install, compile, test and deploy the process application to the Tomcat Server.  Without installing or configuring Java and Mavern on your development machine we can demonstrate this by running the official maven docker image with just a few command line options.  Below is the run command to install, compile, test, and deploy to the Tomcat Server launched above.

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

## Build & Deploy Process Application (Eclipse Option)

Deploy the monthly-meetup process application using Java and maven installed and the development machine host using the eclipse Integrated Development Environment.

* Install [Eclipse IDE For Java Developers - Neon1a](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neon1a)

* Install [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

* Import project into eclipse

 * Open Eclipse

 * File Menu -> Import -> Maven -> Existing Maven Projects -> Next

* Create run configuration to deploy to Tomcat Server

  * Run Menu -> Run Configurations:

    * <img src="/images/run_configuration_1.png">

    * Right Click On Maven and select New

  * Configure a deploy build job

    * <img src="/images/run_configurations_2.png">

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

  * Deploy

   * <img src="/images/run_configurations_2.png">

   * Depending on whether this is the first or 2nd time you've deployed this app to the Tomcat server either run the deploy or redeploy buid.
