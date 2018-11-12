# ejb-multi-server: EJB Communication Across Servers

Author: Wolf-Dieter Fink  
Level: Advanced  
Technologies: EJB, EAR  
Summary: The `ejb-multi-server` quickstart shows how to communicate between multiple applications deployed to different servers using an EJB to log the invocation.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  


## What is it?

The `ejb-multi-server` quickstart demonstrates communication between applications deployed to different Red Hat JBoss Enterprise Application Platform servers. Each application is deployed as an EAR and contains a simple EJB bean. The only function of each bean is to log the invocation.

This example consists of the following Maven projects, each with a shared parent:

| **Sub-project** | **Description** |
|:-----------|:-----------|
| `app-main` | An application that can be called by the `client`. It can also call the different sub-applications. |
| `app-one` and `app-two` | These are simple applications that contain an EJB sub-project to build the `ejb.jar` file and an EAR sub-project to build the `app.ear` file. Each application contains only one EJB that logs a statement on a method call and returns the `jboss.node.name` and credentials. |
| `app-web` |  A simple WAR application. It consists of one Servlet that demonstrates how to invoke EJBs on a different server. |
| `client` | This project builds the standalone client and executes it.|

The root `pom.xml` builds each of the subprojects in an appropriate order.

The server configuration is done using CLI batch scripts located in the root of the quickstart folder.


## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Start with a Clean Server Install

It is important to start with a clean version of JBoss EAP before testing this quickstart. Be sure to unzip or install a fresh JBoss EAP instance.


## Use of EAP7_HOME

In the following instructions, replace `EAP7_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP7_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP7_HOME.md#use-of-eap_home-and-jboss_home-variables).


## Add the Application Users

The following users must be added to the `ApplicationRealm` to run this quickstart. Be sure to use the names and passwords specified in the table as they are required to run this example.

| **UserName** | **Realm** | **Password** | **Roles** |
|:-----------|:-----------|:-----------|:-----------|
| quickuser| ApplicationRealm | quick-123 | _leave blank for none_ |
| quickuser1 | ApplicationRealm | quick123+ | _leave blank for none_ |
| quickuser2 | ApplicationRealm | quick+123 | _leave blank for none_ |

To add the users, open a command prompt and type the following commands:

        For Linux:
            EAP7_HOME/bin/add-user.sh -a -u quickuser -p quick-123
            EAP7_HOME/bin/add-user.sh -a -u quickuser1 -p quick123+
            EAP7_HOME/bin/add-user.sh -a -u quickuser2 -p quick+123

        For Windows:
            EAP7_HOME\bin\add-user.bat -a -u quickuser -p quick-123
            EAP7_HOME\bin\add-user.bat -a -u quickuser1 -p quick123+
            EAP7_HOME\bin\add-user.bat -a -u quickuser2 -p quick+123

If you prefer, you can use the add-user utility interactively. For an example of how to use the add-user utility, see the instructions located here: [Add an Application User](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CREATE_USERS.md#add-an-application-user).

## Back Up the Server Configuration Files

JBoss EAP server configuration for this quickstart is very complicated and not easily restored by running a JBoss CLI script, so it is important to back up your server configuration files before you begin.

1. If it is running, stop the JBoss EAP server.
2. Back up the following files, replacing EAP7_HOME with the path to your JBoss EAP installation:

        EAP7_HOME/domain/configuration/domain.xml
        EAP7_HOME/domain/configuration/host.xml        
3. After you have completed testing and undeployed this quickstart, you can replace these files to restore the server to its original configuration.


## Configure the Server

You configure the domain server by running JBoss CLI commands. For your convenience, this quickstart batches the commands into a `install-domain.cli` script provided in the root directory of this quickstart.

1. Start with a fresh instance of the JBoss EAP as noted above under [Start with a Clean JBoss EAP Install](#start-with-a-clean-server-install).

2. Be sure you add the required users as specified above under [Add the Application Users](#add-the-application-users).

3. Before you begin, make sure you followed the instructions above under [Back Up the JBoss EAP Server Configuration Files](#back-up-the-server-configuration-files).
4.  Start the JBoss EAP server
    * Open a command prompt and navigate to the root of the EAP directory.
    * Start the server using the following command:

            bin/domain.sh    
5. Review the `install-domain.cli` file in the root of this quickstart directory. This script configures and starts multiple servers needed to run this quickstart.

6. Open a new command prompt, navigate to the root directory of this quickstart, and run the following command, replacing EAP7_HOME with the path to your server:

        For Linux: EAP7_HOME/bin/jboss-cli.sh -c --file=install-domain.cli
        For Windows: EAP7_HOME\bin\jboss-cli.bat -c --file=install-domain.cli
     You should see the following result when you run the script:

        {
            "outcome" => "success",
            "result" => "STOPPED"
        }
        {
            "outcome" => "success",
            "result" => "STOPPED"
        }
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined
        }
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined
        }
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined
        }
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined
        }
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined
        }
        The batch executed successfully
        process-state: reload-required
        {
            "outcome" => "success",
            "result" => undefined,
            "server-groups" => undefined,
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "STARTING",
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "STARTING",
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "STARTING",
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "STARTING",
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "STARTING",
            "response-headers" => {"process-state" => "reload-required"}
        }
        {
            "outcome" => "success",
            "result" => "Starting",
            "response-headers" => {"process-state" => "reload-required"}


_NOTE:  Depending on your machine configuration, you may see "Exception in thread "main" java.lang.OutOfMemoryError: unable to create new native thread" exceptions in the server log when you run this script. If you do, you must increase the ulimit open files and max user processes settings. Instructions to do this are located here: <http://ithubinfo.blogspot.com/2013/07/how-to-increase-ulimit-open-file-and.html>. After you update the ulimit settings, be sure to reboot and start with a fresh instance of the server._


## Review the Modified Server Configuration

There are too many additions to the configuration files to list here. Feel free to compare the `domain.xml` and `host.xml` to the backup copies to see the changes made to configure the server to run this quickstart.

## Build and Deploy the Quickstart

1. Make sure you have started and configured the JBoss EAP server successfully   as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type this command to build the artifacts:

        mvn clean install

   You should see `BUILD SUCCESS` at the end of the build `SUCCESS` messages for each component.

4. In the same command prompt, deploy the applications using the provided CLI batch script by typing the following command:

        For Linux: EAP7_HOME/bin/jboss-cli.sh -c --file=deploy-domain.cli
        For Windows: EAP7_HOME\bin\jboss-cli.bat -c --file=deploy-domain.cli

     This will deploy the `app-*.ear` files to different server-groups of the running domain. You should see the following result when you run the script:

        The batch executed successfully
        process-state: reload-required

     You may see the following warnings in the server log. You can ignore these warnings.

        [Server:app-oneB] 13:00:13,346 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 13) JGRP000015: the send buffer of socket MulticastSocket was set to 1MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max send buffer in the OS correctly (e.g. net.core.wmem_max on Linux)
        [Server:app-oneB] 13:00:13,346 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 13) JGRP000015: the receive buffer of socket MulticastSocket was set to 20MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max receive buffer in the OS correctly (e.g. net.core.rmem_max on Linux)
        [Server:app-oneB] 13:00:13,347 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 13) JGRP000015: the send buffer of socket MulticastSocket was set to 1MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max send buffer in the OS correctly (e.g. net.core.wmem_max on Linux)
        [Server:app-oneB] 13:00:13,347 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 13) JGRP000015: the receive buffer of socket MulticastSocket was set to 25MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max receive buffer in the OS correctly (e.g. net.core.rmem_max on Linux)
        [Server:app-oneA] 13:00:13,407 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 11) JGRP000015: the send buffer of socket MulticastSocket was set to 1MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max send buffer in the OS correctly (e.g. net.core.wmem_max on Linux)
        [Server:app-oneA] 13:00:13,408 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 11) JGRP000015: the receive buffer of socket MulticastSocket was set to 20MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max receive buffer in the OS correctly (e.g. net.core.rmem_max on Linux)
        [Server:app-oneA] 13:00:13,408 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 11) JGRP000015: the send buffer of socket MulticastSocket was set to 1MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max send buffer in the OS correctly (e.g. net.core.wmem_max on Linux)
        [Server:app-oneA] 13:00:13,408 WARN  [org.jboss.as.clustering.jgroups.protocol.UDP] (ServerService Thread Pool -- 11) JGRP000015: the receive buffer of socket MulticastSocket was set to 25MB, but the OS only allocated 212.99KB. This might lead to performance problems. Please set your max receive buffer in the OS correctly (e.g. net.core.rmem_max on Linux)


_NOTE:_ If ERRORs appear in the `server.log` when installing or deploying the quickstart, stop the domain and restart it. This should ensure further steps run correctly.

## Access the Remote Client Application

This example shows how to invoke an EJB from a remote standalone application.

1. Make sure that the deployments are successful as described above.
2. Navigate to the quickstart `client/` subdirectory.
3. Type this command to run the application:

        mvn exec:java

    The client will output the following information provided by the applications:

        InvokeAll succeed: MainApp[anonymous]@master:app-main  >  [ app1[quickuser1]@master:app-oneB > app2[quickuser2]@master:app-twoA ; app2[quickuser2]@master:app-twoA ]

    This output shows that the `MainApp` is called with the user `anonymous` at node `master:app-main` and the sub-call is proceeded by the `master:app-oneA` node and `master:app-twoA` node as `quickuser2`.

    Review the server log files to see the bean invocations on the servers.

    __Note__: This quickstart requires `quickstart-parent` artifact to be installed in your local Maven repository.
    To install it, navigate to quickstarts project root directory and run the following command:

        mvn clean install

4. If it is necessary to invoke the client with a different JBoss EAP version the main class can be invoked by using the following command from the root directory of this quickstart. Replace EAP7_HOME with your current installation path. The output should be similar to the previous mvn executions.

        java -cp EAP7_HOME/bin/client/jboss-client.jar:app-main/ejb/target/ejb-multi-server-app-main-ejb-client.jar:app-two/ejb/target/ejb-multi-server-app-two-ejb-client.jar:client/target/ejb-multi-server-client.jar org.jboss.as.quickstarts.ejb.multi.server.Client


_NOTE:_

* _If exec is called multiple times, the invocation for `app1` might use `app-oneA` and `app-oneB` node due to cluster loadbalancing._
* _A JBoss EAP will deny the invocation of unsecured methods of `appOne`/`appTwo` since security is enabled but the method does not include @Roles. You need to set `default-missing-method-permissions-deny-access = false` for the `ejb3` subsystem within the domain profile `ha` and `default` to allow the method invocation. See the `install-domain.cli` script._


## Access the JSF application Inside the Main Application

The JSF example shows different annotations to inject the EJB. Also how to handle the annotation if different beans implement the same interface and therefore the container is not able to decide which bean needs to be injected without additional informations.

1. Make sure that the deployments are successful as described above.
2. Use a browser to access the JSF application at the following URL: <http://localhost:8080/ejb-multi-server-app-main-web/>
3. Insert a message in the Text input and invoke the different methods. The result is shown in the browser.
4. See server logfiles and find your given message logged as INFO.

## Access the Servlet Application Deployed as a WAR Inside a Minimal Server

An example how to access EJBs from a separate instance which only contains a web application.

1. Make sure that the deployments are successful as described above.
2. Use a browser to access the Servlet at the following URL: <http://localhost:8380/ejb-multi-server-app-web/>
3. The Servlet will invoke the remote EJBs directly and show the results, compare that the invocation is successful


## Undeploy the Archives

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        For Linux: EAP7_HOME/bin/jboss-cli.sh --connect --file=undeploy-domain.cli
        For Windows: EAP7_HOME\bin\jboss-cli.bat --connect --file=undeploy-domain.cli


## Remove the Server Domain Configuration

1. If it is running, stop the JBoss EAP server.
2. Restore the `EAP7_HOME/domain/configuration/domain.xml` and `EAP7_HOME/domain/configuration/host.xml` files with the back-up copies of the files. Be sure to replace EAP7_HOME with the path to your server.


## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

EJB Client (ejb-client) currently has limited support in the Eclipse Web Tools Platform (WTP). For that reason, this quickstart is not supported in Red Hat JBoss Developer Studio.

## Debug the Application

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

    mvn dependency:sources
