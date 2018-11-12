# hibernate: How to Use Hibernate 5 in an Application

Author: Madhumita Sadhukhan  
Level: Intermediate  
Technologies: Hibernate  
Summary: The `hibernate` quickstart demonstrates how to use Hibernate ORM 5 API over JPA, using Hibernate-Core and Hibernate Bean Validation, and EJB.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The `hibernate` quickstart is based upon the [kitchensink](../kitchensink/README.md) example, but demonstrates how to use Hibernate Object/Relational Mapping (ORM) 5 over JPA in Red Hat JBoss Enterprise Application Platform.

This project is setup to allow you to create a compliant Java EE 7 application using JSF, CDI, EJB, JPA , Hibernate-Core and Hibernate Bean Validation.  It includes a persistence unit associated with Hibernate session and some sample persistence and transaction code to help you with database access in enterprise Java.

_Note: This quickstart uses the H2 database included with Red Hat JBoss Enterprise Application Platform 7.1. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable, is not supported, and should NOT be used in a production environment!_

_Note: This quickstart uses a `*-ds.xml` datasource configuration file for convenience and ease of database configuration. These files are deprecated in JBoss EAP and should not be used in a production environment. Instead, you should configure the datasource using the Management CLI or Management Console. Datasource configuration is documented in the [Configuration Guide](https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/) for Red Hat JBoss Enterprise Application Platform._

## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Use of EAP7_HOME

In the following instructions, replace `EAP7_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP7_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP7_HOME.md#use-of-eap_home-and-jboss_home-variables).


## Add the Correct Dependencies

JBoss EAP provides Hibernate 5 and JPA support.

If you use Hibernate 5 packaged within JBoss EAP, you will need to first import the JPA API.

This quickstart demonstrates usage of Hibernate Session and Hibernate Validators.

If you look at the pom.xml file in the root of the hibernate quickstart directory, you will see that the dependencies for the Hibernate modules have been added with the scope as `provided`.
For example:

      <dependency>
         <groupId>org.hibernate</groupId>
         <artifactId>hibernate-validator</artifactId>
          <scope>provided</scope>
         <exclusions>
            <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
            </exclusion>
         </exclusions>
      </dependency>


## Start the Server

1. Open a command prompt and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


## Build and Deploy the Quickstart

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean install wildfly:deploy

4. This will deploy `target/hibernate.war` to the running instance of the server.


## Access the Application

The application will be running at the following URL: <http://localhost:8080/hibernate/>.


## Server Log: Expected Warnings and Errors

_Note:_ You will see the following warnings in the server log. You can ignore these warnings.

    WFLYJCA0091: -ds.xml file deployments are deprecated. Support may be removed in a future version.

    HHH000431: Unable to determine H2 database version, certain features may not work


## Undeploy the Archive

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy


## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts). 


## Debug the Application

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

        mvn dependency:sources

You may see the following message when you run the command. It indicates the source is not provided in the third-party `antlr` JAR.

        [INFO] The following files have NOT been resolved:
        [INFO]    antlr:antlr:jar:sources:2.7.7:provided
