# spring-kitchensink-springmvctest: Kitchensink MVC Example Using Spring 4.x

Author: Marius Bogoevici, Tejas Mehta, Joshua Wilson  
Level: Intermediate  
Technologies: JSP, JPA, JSON, Spring, JUnit  
Summary: The  `spring-kitchensink-springmvctest` quickstart demonstrates how to create an MVC application using JSP, JPA and Spring 4.x.   
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The  `spring-kitchensink-springmvctest` quickstart demonstrates how to create an MVC application using JSP, JPA and Spring 4.x in Red Hat JBoss Enterprise Application Platform.

* This module adds `MemberMockMVCTest.java` to showcase a use case of `MockMVC` and `RestTemplate` to test the MVC aspect of the application.

* By using `@WebAppConfiguration` and `@ContextConfiguration` we tell Spring the configuration files we would like the tests to use.

* In `jboss-as-spring-mvc-context.xml` `<context:component-scan base-package="org.jboss.as.quickstarts.kitchensink.spring.springmvctest.controller"/>`
and `<mvc:annotation-driven/>` are used to register both the non-rest and rest controllers.

* The controllers map the respective urls to methods using `@RequestMapping(url)`.

* To return JSON, the rest controller uses `@ResponseBody`.

* The datasource and entitymanager are retrieved via JNDI.


## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Start the Server

1. Open a command line and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


## Build and Deploy the Quickstart

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command line and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean install wildfly:deploy

4. This will deploy `target/spring-kitchensink-springmvctest.war` to the running instance of the server.


## Access the Application

The application will be running at the following URL: <http://localhost:8080/spring-kitchensink-springmvctest/>.


## Undeploy the Archive

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command line and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy


## Run the Arquillian Functional Tests

This quickstart provides Arquillian functional tests as well. They are located in the functional-tests/ subdirectory under
the root directory of this quickstart. Functional tests verify that your application behaves correctly from the user's point
of view. The tests open a browser instance, simulate clicking around the page as a normal user would do, and then close the browser instance.

To run these tests, you must build the main project as described above.

1. Open a command line and navigate to the root directory of this quickstart.
2. Build the quickstart WAR using the following command:

        mvn clean package

3. Navigate to the functional-tests/ directory in this quickstart.
4. If you have a running instance of the JBoss EAP server, as described above, run the remote tests by typing the following command:

        mvn clean verify -Parq-remote

5. If you prefer to run the functional tests using managed instance of the JBoss EAP server, meaning the tests will start the
server for you, type the following command:

        mvn clean verify -Parq-managed


## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts).


## Debug the Application

If you want to debug the source code or look at the Javadocs of any library in the project, run either of the following
commands to pull them into your local repository. The IDE should then detect them.

        mvn dependency:sources
        mvn dependency:resolve -Dclassifier=javadoc
