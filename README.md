# javaee7-angular-openshift
# Java EE 7 - Angular - on Openshift with Postgres database - Sample Application #

## Overview ##

This is an adaptation to openshift of an example shown in a short series of post made by Roberto Cortez in 2014 [1](http://www.radcortez.com/java-ee-7-with-angular-js-part-1), [2](http://www.radcortez.com/java-ee-7-with-angular-js-crud-rest-validations-part-2), [3](http://www.radcortez.com/codenvy-setup-to-demo-applications-using-docker-java-ee-7-with-angular/). 

The main motivation for adapting this to openshift from codenvy is its great potential and huge flexibility. The idea that in just jew clicks, one can get a fully scalable application server ready to go is perfect. Moreover, I have not seen it being done in the net, so it might be useful for some people. There are some minor changes to orginal Cortez's work. 

## Changes ##

1. Intruduction of import.sql for Hibernate. This file is loaded up during the start of the webapp, and is used by the hibernate to initialize the database. Very handy tool for debugging and quick testing. It is also a source of an error, i.e., onec a new entity is created, hibernate tries to use the first free in his eyes, id value which is already occupied. A workaround would be to inject entites on start from within the session bean, or use some initialization techniqe which uses the exposed logic. Under fixing.

2. Modification of persistance.xml file. By default the persistance of data is provided by the Wildfly via internal H2 database. What is H2? It is an opensource lightweight Java database which is ships with JBoss AS distribution. Whilst it's not recommended for production usage, it can be pretty useful for development/test with a more advanced configuration. In the example, I wanted to use something bit closer to real applications, therefore Postgres database. The modification made in the persistance.xml file covered indication of the datasource and, removal of non-hibernate specific annotations which seemed not to work with Postgres. Other option why they did not worked might be the capital letters used in the Cortez's sql files example.

3. As mentioned, the change in the sql files, to small letters. Once done it worked out-of-the-box.

4. I have not managed to get javascript build tools running. Something was wrong with the output provided by grunt therefore I skipped it. If anyone finds out how to fix it - let me know.

## Openshift ##

First of all, one needs to get the openshift account and create a remote application using the provided WildFly Application Server 8.2.1.Final. A good descrpition can be found [here](https://github.com/openshift-cartridges/openshift-wildfly-cartridge). Next, one needs to add PostgreSQL 9.2 database to the running service. It is described [here](https://developers.openshift.com/managing-your-applications/adding-a-database.html).

## Localhost ##

* You need JDK 7 or higher, Maven 3 to compile the application.
* Once you cloned the project using git then simply use maven to build it using 

        `mvn clean install`. 

### Deploy to remote server ###

The idea it to build the project locally but test is in the remote server.
* In order to deploy the webapp we simply copy it via secure copy to the openshift webserver. 

        scp ./target/javaee7-angular.war <SSH-ID>@<myappname>.rhcloud.com:~/wildfly/standalone/deployments

If you encounter any problems, try finding out how to ssh on your openshift server. Details can be found [here](https://developers.openshift.com/managing-your-applications/remote-connection.html).

