# Camunda-Lab-1

**Objective:** Create, build and deploy a new process application to a Tomcat Server setup with [Camunda shared-engine architecture](https://docs.camunda.org/manual/7.4/introduction/architecture/#shared-container-managed-process-engine)

<img src="images/overview.png">

# Run a Tomcat Server setup with Camunda shared-engine architecture

## Summary

1. Clone Lab and Setup

2. Run Tomcat Server

3. Verify Tomcat Server can accept Process Application Deployments

## Clone Lab and Setup

This project uses the variable $LABS_HOME to refer to the folder where you've cloned this repo.

```
export LABS_HOME=/users/jrhicks/labs
```

Checkout this repo

```
cd $LABS_HOME
git clone https://github.com/jrhicks/camunda-lab-1.git
```

This lab requires [docker engine installation](https://docs.docker.com/engine/installation/).

## Run Tomcat Server

Docker is used to build a Tomcat server with system dependencies, some general camunda web applications, and configurations useful for this lab.

```
cd tomcat-server
docker build . -t camunda
docker run -p 8080:8080 camunda
```

## Verify Tomcat Server can accept Process Application Deployments (Using Docker)

Docker is not necessary to build and deploy a process application.  However, since the docker method is very reproducible it serves as a good mechanism to verify the Tomcat Server is setup correctly to accept deployments.

```
cd camunda-apps/monthly-meetup

docker run -it --rm \
       -v "$(pwd)":/opt/maven \
       -w /opt/maven \
       --net="host" \
       maven:3.2-jdk-8 \
       mvn -s settings.xml clean tomcat7:deploy
```

Confirm Deployment by opening [http://localhost:8080](http://localhost:8080), create user, login, start process.

# Create, Build and Deploy an existing process application using Eclipse IDE

Ultimately we want to create our own process applications to deploy.  First we setup our Eclipse IDE and verify correct setup by deploying an existing process application.

## Summary

1. Install dependencies

2. Import existing project into Eclipse

3. Update user maven settings with Tomcat Server credentials

4. Create deploy & redeploy run configurations

5. Deploy

## Install dependencies

1. Install [Eclipse IDE For Java Developers - Neon1a](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neon1a)

2. Install [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

3. Install Maven

 * [Mac Brew](http://brewformulas.org/Maven) - brew install maven

 * [Manually on Mac, Windows or Linux](https://maven.apache.org/install.html)

## Import existing project into Eclipse

1. Open Eclipse

2. File Menu -> Import -> Maven -> Existing Maven Projects -> Next

## Create deploy & redeploy run configurations

1. Run Menu -> Run Configurations

<table><tr><td>
<img src="/images/run_configurations_1.png">
</td><td valign="top">
<ol>
<li> Right Click On Maven -> New
<li> Name: deploy
<li> Base directory: ${project_loc}
<li> Goals: tomcat7:deploy
<li> Note the path of user settings.  In later steps we will refer to this as $USER_SETTINGS_PATH
<li> Apply
<li> Close
</td></tr></table>

Configure a redeploy build job.  Repeat previous steps to create a run configuration except name it redeploy and set the goal to: tomcat7:redeploy.

## Update user maven settings

Copy user_settings.xml into $USER_SETTINGS_PATH

```
export USER_SETTINGS_PATH=~/.m2/user_settings.xml
```

```
cp $LABS_HOME/camunda_lab_1/camunda-apps/monthly-meetup/settings.xml $USER_SETTINGS_PATH
```

## Deploy

* Run Build -> Depending on situation either deploy or redeploy.

# Create, Build and Deploy a new process application

<img src="images/overview2.png">

Camunda provides [Maven Project Templates (Archetypes)](https://docs.camunda.org/manual/7.4/user-guide/process-applications/maven-archetypes/) to enable a quick start to developing process applications.

## Summary

1. Add Camunda Project Template Catalog to Eclipse

2. Create a Process Application Project Using Template

3. Configure Run Configurations

4. Edit Process using Camunda Modeler

5. Build & Deploy

## Add Camunda Project Template Catalog to Eclipse

1. Go to **Preferences -> Maven -> Archetypes -> Add Remote Catalog**
<img src="/images/eclipse-00-preferences-maven-archetypes.png" title="Eclipse Preferences: Maven Archetypes" >

2. Enter the following URL and description, click on **Verify...** to test the connection and if that worked click on **OK** to save the catalog.

    Catalog File: **https://app.camunda.com/nexus/content/repositories/camunda-bpm/**

    Description: **camunda BPM platform**

<img src="/images/eclipse-01-add-remote-archetype-catalog.png" title="Eclipse Preferences: Add Maven Archetype Catalog" >

## Create a Process Application Project Using Template

Now you should be able to use the archetypes when creating a new Maven project in Eclipse:

1. Go to **File -> New -> Project...** and select **Maven -> Maven Project**

2. Select a location for the project or just keep the default setting.

3. Select the Catatolog you created before, and select the **camunda-archetype-servlet-war**

4. Specify Archetype parameters and finish
<img src="images/finish-new-maven-project.png" title="New Maven Project: Specify Archetype Parameters">

## Configure Run Configurations

Update Project with Tomcat Server Credentials.  Right click on pom.xml and open with Text Editor.  Locate near line 185:

```
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.2</version>
  <configuration>
    <url>http://localhost:8080/manager/text</url>
    <username>admin</username>
    <password>admin</password>
  </configuration>
</plugin>
```

Replace the above exposed username and password with a reference to the TomcatServer whose credentials are located in user_settings.xml

```
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.2</version>
  <configuration>
    <url>http://localhost:8080/manager/text</url>
    <server>TomcatServer</server>
  </configuration>
</plugin>
```

Create deploy and redeploy run configurations

The previous instructions **create deploy & redeploy run configurations** used the ${project_loc} variable for the base directory - so you can use those same run configurations for this newly created project.  Unless previously completed, do that now.

## Edit Process using Camunda Modeler

1. [Download camunda modeler](https://camunda.org/download/modeler/)

2.  

... to be continued
