# kitchensink-ml: Localized Version of the kitchensink Quickstart

Author: Sande Gilda  
Level: Intermediate  
Technologies: CDI, JSF, JPA, EJB, JAX-RS, BV, i18n, l10n  
Summary: The `kitchensink-ml` quickstart demonstrates a localized Java EE 7 compliant application using JSF, CDI, EJB, JPA, and Bean Validation.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

## What is it?

The `kitchensink-ml` quickstart is a deployable Maven 3 project to help you get your foot in the door developing with Java EE 7 on Red Hat JBoss Enterprise Application Platform.

It demonstrates how to create a _localized_ Java EE 7 compliant application using JSF, CDI, JAX-RS, EJB, JPA, and Bean Validation. A localized application is one that supports multiple languages. That is what the _-ml_ suffix denotes in the quickstart name _kitchensink-ml_. This quickstart also includes a persistence unit and some sample persistence and transaction code to introduce you to database access in enterprise Java.

This quickstart uses the [kitchensink](../kitchensink/README.md) quickstart as its starting point. It has been enhanced to provide localization of labels and messages. A user sets the preferred language choice in the browser and, if the application supports that language, the application web page is rendered in that language. For demonstration purposes, this quickstart has been tranlated into French(fr) and Spanish (es) using <http://translate.google.com>, so the translations may not be ideal.

_Note: This quickstart uses the H2 database included with Red Hat JBoss Enterprise Application Platform 7.1. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable, is not supported, and should NOT be used in a production environment!_

_Note: This quickstart uses a `*-ds.xml` datasource configuration file for convenience and ease of database configuration. These files are deprecated in JBoss EAP and should not be used in a production environment. Instead, you should configure the datasource using the Management CLI or Management Console. Datasource configuration is documented in the [Configuration Guide](https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/) for Red Hat JBoss Enterprise Application Platform._

### Localization Code Changes

The following changes were made to the quickstart to enable it to use the browser preferred locale setting when displaying the web page:

* Properties files were created for the supported languages.

    * This quickstart is localized for Spanish  and  French. You can add additional language support by creating properties files with the appropriate suffix and populating the properties with translated values.

    * The JSF resource Bundle is located at `src/main/resources/org/jboss/as/quickstarts/kitchensink/bundle/Resources_(es|fr).properties

    * Messages generated by Java code (e.g. log messages and messages sent to the UI) are internationalized using JBoss Logging. The log messages are accessed via the `org.jboss.as.quickstarts.kitchensink.util.KitchensinkMessages` interface, and the message bundles are located at: `src/main/resources/org/jboss/as/quickstarts/kitchensink/util/KitchensinkMessages.i18_(es|fr).properties`

    * The message bundle consumed by Bean Validation is located at `src/main/resources/ValidationMessages.properties`. This is defined by the bean validation specification.

* The following XML was added to the `src/main/webapp/WEB-INF/faces-config.xml` file. When you create a property file for a new language, you must add the supported locale to this file.

        <application>
          <locale-config>
            <default-locale>en</default-locale>
            <supported-locale>en-US</supported-locale>
            <supported-locale>fr</supported-locale>
            <supported-locale>fr-FR</supported-locale>
            <supported-locale>es</supported-locale>
            <supported-locale>es-ES</supported-locale>
          </locale-config>
          <resource-bundle>
            <base-name>org/jboss/as/quickstarts/kitchensink/bundle/Resources</base-name>
            <var>bundle</var>
          </resource-bundle>
        </application>

* The `src/main/java/org/jboss/as/quickstarts/kitchensink/model/Member.java` file was modififed to add the message key to @Pattern annotation.

         @NotNull
         @Size(min = 1, max = 25)
         @Pattern(regexp = "[A-Za-z ]*", message = "{name_validation_message}")
         private String name;                

* The `src/main/java/org/jboss/as/quickstarts/kitchensink/util/KitchensinkMessages.java` file was created, which defines default messages in English. The `jboss-logging-processor` will automatically generate an implementation for you, which can be accesssed via the `MESSAGES` static variable.

        @MessageBundle(projectCode = "")
        public interface KitchensinkMessages {

           KitchensinkMessages MESSAGES = Messages.getBundle(KitchensinkMessages.class, FacesContext.getCurrentInstance().getViewRoot().getLocale());

           @Message("Registered!")
           String registeredMessage();

           @Message("Successfully registered!")
           String registerSuccessfulMessage();

           @Message("Registration failed:")
           String registerFailMessage();

           @Message("Registration failed. See server log for more information.")
           String defaultErrorMessage();
        }

* The `src/main/java/org/jboss/as/quickstarts/kitchensink/controller/MemberController.java` file was modified as follows:

    * Messages strings were replaced with strings retrieved using the resource bundle property names. For example:

            FacesMessage m = new FacesMessage(FacesMessage.SEVERITY_INFO,
                    KitchensinkMessages.MESSAGES.registeredMessage(),
                    KitchensinkMessages.MESSAGES.registerSuccessfulMessage());

* The `src/main/webapp/index.xhtml` file were modified.

    * Strings for headers, messages, labels were replaced with the appropriate `# {bundle.<property>}`, for example: `# {bundle.memberWelcomeHeader}`.

### Set the Browser Preferred Locale

How you set your browser preferred locale depends on the browser and version you use. Use your browser help option to search for instructions to change the preferred language setting.



## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


##  Use of EAP7_HOME

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

4. This will deploy `target/kitchensink-ml.war` to the running instance of the server.


## Access the Application

The application will be running at the following URL: <http://localhost:8080/kitchensink-ml/>.

Change your browser preferred language to French or Spanish and refresh the page to see it displayed in the new language.


## Server Log: Expected Warnings and Errors

_Note:_ You will see the following warnings in the server log. You can ignore these warnings.

    WFLYJCA0091: -ds.xml file deployments are deprecated. Support may be removed in a future version.

    HHH000431: Unable to determine H2 database version, certain features may not work

## Undeploy the Archive

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy


## Run the Arquillian Tests

This quickstart provides Arquillian tests. By default, these tests are configured to be skipped as Arquillian tests require the use of a container.

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type the following command to run the test goal with the following profile activated:

        mvn clean verify -Parq-remote

You can also let Arquillian manage the JBoss EAP server by using the `arq-managed` profile. For more information about how to run the Arquillian tests, see [Run the Arquillian Tests](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/RUN_ARQUILLIAN_TESTS.md#run-the-arquillian-tests).


## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts).


## Debug the Application

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

    mvn dependency:sources