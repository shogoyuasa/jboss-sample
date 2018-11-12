# ejb-timer: Example of EJB Timer Service - @Schedule and @Timeout
Author: Ondrej Zizka <ozizka@redhat.com>  
Level: Beginner  
Technologies: EJB Timer  
Summary: The `ejb-timer` quickstart demonstrates how to use the EJB timer service `@Schedule` and `@Timeout` annotations with JBoss EAP.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The `ejb-timer` quickstart demonstrates how to use the EJB timer service in Red Hat JBoss Enterprise Application Platform. This example creates a timer service that uses the `@Schedule` and `@Timeout` annotations.


The following EJB Timer services are demonstrated:

 * `@Schedule`: Uses this annotation to mark a method to be executed according to the calendar schedule specified in the attributes of the annotation. This example schedules a message to be printed to the server console every 6 seconds.
 * `@Timeout`: Uses this annotation to mark a method to execute when a programmatic timer goes off. This example sets the timer to go off every 3 seconds, at which point the method prints a message to the server console.


## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Use of EAP7_HOME

In the following instructions, replace `EAP7_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP7_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP7_HOME.md#use-of-eap_home-and-jboss_home-variables).


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

This will deploy `target/ejb-timer.war` to the running instance of the server.


## Access the Application

This application only prints messages to stdout.
To see it working, check the server log. You should see similar output:

    INFO  [stdout] (EJB default - 10) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:12 EST
    INFO  [stdout] (EJB default - 2) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:12 EST
    INFO  [stdout] (EJB default - 4) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:15 EST
    INFO  [stdout] (EJB default - 3) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:18 EST
    INFO  [stdout] (EJB default - 5) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:18 EST
    INFO  [stdout] (EJB default - 7) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:21 EST
    INFO  [stdout] (EJB default - 9) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:24 EST
    INFO  [stdout] (EJB default - 6) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:24 EST
    INFO  [stdout] (EJB default - 8) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:27 EST
    INFO  [stdout] (EJB default - 1) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:30 EST
    INFO  [stdout] (EJB default - 10) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:30 EST
    INFO  [stdout] (EJB default - 2) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:33 EST
    INFO  [stdout] (EJB default - 4) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:36 EST
    INFO  [stdout] (EJB default - 3) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:36 EST
    INFO  [stdout] (EJB default - 5) TimeoutExample.scheduler() EJB timer service timeout at 2014.11.25 AD at 11:57:39 EST
    INFO  [stdout] (EJB default - 7) ScheduleExample.doWork() invoked at 2014.11.25 AD at 11:57:42 EST

Existing threads in the thread pool handle the invocations. They are rotated and the name of the thread that handles the invocation is printed within the parenthesis `(EJB Default - #)`.


## Undeploy the Archive

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy

## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts).


## Debug the Application

If you want to debug the source code of any library in the project,
run the following command to pull the source into your local repository. The IDE should then detect it.

        mvn dependency:sources
