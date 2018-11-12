# spring-resteasy: Example Using Resteasy Spring Integration

Author: Weinan Li <l.weinan@gmail.com>, Paul Gier <pgier@redhat.com>  
Level: Beginner  
Technologies: Resteasy, Spring  
Summary: The `spring-resteasy` quickstart demonstrates how to package and deploy a web application that includes resteasy-spring integration.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The `spring-resteasy` quickstart demonstrates how to package and deploy a web application, which includes resteasy-spring integration, in
Red Hat JBoss Enterprise Application Platform.

## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Start the Server

1. Open a command line and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server with the full profile:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


## Build and Deploy the Quickstart

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command line and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean package wildfly:deploy

4. This deploys the `target/spring-resteasy.war` to the running instance of the server.


## Access the Application

The application will be running at the following URL:  <http://localhost:8080/spring-resteasy/>.

That will provide links to the following URLs that demonstrate various path and parameter configurations.

* [spring-resteasy/hello?name=yourname](http://localhost:8080/spring-resteasy/hello?name=yourname)
* [spring-resteasy/basic](http://localhost:8080/spring-resteasy/basic)
* [spring-resteasy/queryParam?param=query](http://localhost:8080/spring-resteasy/queryParam?param=query)
* [spring-resteasy/matrixParam;param=matrix](http://localhost:8080/spring-resteasy/matrixParam;param=matrix)
* [spring-resteasy/uriParam/789](http://localhost:8080/spring-resteasy/uriParam/789)

And the same set as above but using the `locating` path.

* [spring-resteasy/locating/hello?name=yourname](http://localhost:8080/spring-resteasy/locating/hello?name=yourname)
* [spring-resteasy/locating/basic](http://localhost:8080/spring-resteasy/locating/basic)
* [spring-resteasy/locating/queryParam?param=query](http://localhost:8080/spring-resteasy/locating/queryParam?param=query)
* [spring-resteasy/locating/matrixParam;param=matrix](http://localhost:8080/spring-resteasy/locating/matrixParam;param=matrix)
* [spring-resteasy/locating/uriParam/789](http://localhost:8080/spring-resteasy/locating/uriParam/789)

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
