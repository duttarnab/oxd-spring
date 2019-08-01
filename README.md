# oxd-spring <!-- intro -->
### Table of Contents 
1. [About](https://github.com/GluuFederation/oxd-spring#about)
2. [How it works](https://github.com/GluuFederation/oxd-spring#how-it-works)
3. [Run demo application](https://github.com/GluuFederation/oxd-spring#run-demo-application)
    1. [Prerequisites](https://github.com/GluuFederation/oxd-spring#prerequisites)
    2. [Deploy](https://github.com/GluuFederation/oxd-spring#deploy)
4. [Customize](https://github.com/GluuFederation/oxd-spring#customize-oxd-spring)

# About
The following documentation demonstrates how to use Gluu's commercial OAuth 2.0 client software, 
[oxd 4.0 Beta](https://gluu.org/docs/oxd/), to send users from a Spring application to an OpenID Connect Provider 
(OP) for login. You can send users to any standard OP for login, including Google. 
In these docs we use the [free open source Gluu Server](https://gluu.org/docs/ce/4.0/) as the OP.

# How it works
What happens when application is started.

    The first time you run the application, it tries to register site using the parameters specified in [application.properties](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties). If registration was successful, then [oxd.server.op-host](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L19) and received from oxd server `oxdId` are stored in the H2 database (which is embedded in oxd-spring-0.0.1-SNAPSHOT.jar). Next time you run the application with the same [oxd.server.op-host](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L19), it will obtain `oxdId` from database. [Here](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/Settings.java#L41) is the source code that does this.
    
# Run demo application

## Prerequisites

1. Java 8+

Install [Java Standard Edition](http://www.oracle.com/technetwork/java/javase/downloads/2133151) version 8 or higher.

2. Maven 3+

Download [maven](https://maven.apache.org/download.cgi) and follow the simple installation instructions. Ensure the `bin` directory is added to your PATH.

3. An OpenID Connect Provider (OP), like the Gluu Server

Learn how to download and install Gluu server by visiting this [page](https://gluu.org/docs/ce/installation-guide/).

4. oxd-server - 4.0

Download and install [oxd-server 4.0](https://gluu.org/docs/oxd/4.0/).

## Deploy
Clone oxd-spring from Github repo and run maven command to install it:
```
git clone https://github.com/GluuFederation/oxd-spring.git
cd oxd-spring 
mvn clean package -Dmaven.test.skip=true
```
Make sure that oxd server is running. Now you can run the executable jar:
```
java -jar target/oxd-spring-0.0.1-SNAPSHOT.jar
```

Point the browser to `https://127.0.0.1:8080/`. 

## Customize oxd-spring
By default [oxd-spring](https://github.com/GluuFederation/oxd-spring) uses [ce-dev.gluu.org](https://ce-dev.gluu.org) as openid provider, however you can easelly point it at your own server. There are two ways you can do it:
1. Modify [oxd.server.op-host](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L19) property, e.g:
    ```
    oxd.server.op-host=https://your.gluu.server
    ```
    Build and run the application by running the following commands:
    ```
    mvn clean package -Dmaven.test.skip=true
    java -jar target/oxd-spring-0.0.1-SNAPSHOT.jar
    ```
2. Or you can just launch the application with additional parameter, e.g:
    ```
    java -jar target/oxd-spring-0.0.1-SNAPSHOT.jar --oxd.server.op-host=https://your.gluu.server
    ```
[oxd-spring](https://github.com/GluuFederation/oxd-spring) also allows configuring other parameters:
- [oxd.server.scopes](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L23) a list of comma separated scopes, this parameter used during [client registration](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L55) and [obtaining login url](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L83)

- [oxd.server.acr-values](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L22) a list of comma separated acr values, this parameter used during [client registration](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L55) and [obtaining login url](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L83)

- [oxd.server.port](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L21) used to [create connection](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L43) with oxd server

- [oxd.server.host](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L20) used to [create connection](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/java/org/xdi/oxd/spring/service/OxdServiceImpl.java#L43) with oxd server

These parameters could be modified in the same way as [oxd.server.op-host](https://github.com/GluuFederation/oxd-spring/blob/master/src/main/resources/application.properties#L19), e.g:
```
java -jar target/oxd-spring-0.0.1-SNAPSHOT.jar --oxd.server.host=https://your.gluu.server --oxd.server.port=your.port --oxd.server.acr-values=basic,u2f --oxd.server.scopes=openid,profile,email,phone
```
