# payment-cdi-event: Use CDI Events to Process Debit and Credit Operations

Author: Elvadas Nono  
Level: Beginner  
Technologies: CDI, JSF  
Summary: The `payment-cdi-event` quickstart demonstrates how to create credit and debit *CDI Events* in JBoss EAP, using a JSF front-end client.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The `payment-cdi-event` quickstart demonstrates how to use *CDI Events* in Red Hat JBoss Enterprise Application Platform.

The JSF front-end client allows you to create both credit and debit operation events.

To test this quickstart, enter an amount, choose either a Credit or Debit operation, and then click on *Pay* to create the event.

A Session scoped (`@SessionScoped`) payment event handler catches the operation and produces (`@Produces`) a named list of all operations performed during this session. The event is logged in the JBoss EAP server log and the event list is displayed in a table at the bottom of the form.

The `payment-cdi-event` quickstart defines the following classes and interfaces:

* The `beans` package contains the `PaymentBean` bean class:
   * This is a session scoped bean that stores the payment form information:
       * payment amount
       * operation type: debit or credit
   * It contains the following utility methods:
       * `private void init()`: This is a PostConstruct (`@PostConstruct`) method that performs initialization before the class is put into service. It resets the `amount` to `$0` and the `paymentOption` to the default type of debit.
       * `public String pay()`: This method processes the operation when the user clicks on submit. We have only one JSF page, so the method does not return anything and the flow of control does not change.
       * `public void reset()`: Reset calls the `init()` method reinitialize the form values.
* The `events` package contains the `PaymentEvent` class and the enum `PaymentTypeEnum`.
  * `PaymentEvent`: We have only one event that handles both credit and debit operations. Qualifiers help us to make the difference at injection point.
  * `PaymentTypeEnum`:  A typesafe enum is used to represent the operation payment type. It contains utility methods to convert between `String` and `Enum`.
* The `qualifiers` package contains the `Credit` and `Debit` interfaces. The annotation determines the operation of the injecting `Event`.
* The `handler` package contains interfaces and implementations of `PaymentEvent` observers.
  * `ICreditEventObserver`: Interface to listen to `CREDIT` event only (`@Observes` `@Credit`).
  * `IDebitEventObserver`: Interface to listen to `DEBIT` event (`@Observes` `@Debit`).
  * `PaymentHandler`: The concrete implementation of the payment handler.
    * It implements both `ICreditEventObserver` and `IDebitEventObserver`.
    * The payment handler exposes the list of events caught during a session (`@Named`  `name=payments`).
* The `resources` package contains the `Resources` class that produces the logger for the application.


## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Use of EAP7_HOME

In the following instructions, replace `EAP7_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP7_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP7_HOME.md#use-of-eap_home-and-jboss_home-variables).


## Start the JBoss EAP Server

1. Open a command prompt and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


## Build and Deploy the Quickstart

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean install wildfly:deploy

4. This will deploy `target/payment-cdi-event.war` to the running instance of the server.


## Access the Application

The application will be running at the following URL: <http://localhost:8080/payment-cdi-event/>.


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
