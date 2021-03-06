# Example with Undertow, Weld, Jersey and WebSockets

[![Build Status](https://travis-ci.org/cassiomolin/jersey-websockets-undertow.svg?branch=master)](https://travis-ci.org/cassiomolin/jersey-websockets-undertow)
[![MIT Licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/cassiomolin/jersey-websockets-undertow/master/LICENSE.txt)

Example of application using:

- **Undertow:** Servlet container that also provides WebSockets support (JSR 356 implementation).
- **Weld:** CDI reference implementation.
- **Jersey:** JAX-RS reference implementation for creating RESTful web services in Java.
- **WebSockets:** Using the JSR 356 implementation provided by Undertow.

In this example, a message created from the REST API is broadcasted to all WebSocket clients. [CDI events][] are used to send data from the REST API to the WebSocket endpoint.

## Building and running this application

Follow these steps to build and run this application:

1. Open a command line window or terminal.
1. Navigate to the root directory of the project, where the `pom.xml` resides.
1. Compile the project: `mvn clean compile`.
1. Package the application: `mvn package`.
1. Change into the `target` directory: `cd target`
1. You should see a file with the following or a similar name: `jersey-websockets-undertow-1.0.jar`.
1. Execute the JAR: `java -jar jersey-websockets-undertow-1.0.jar`.
1. A page to test the application will be available at `http://localhost:8080/index.html`. The following endpoints will be available:
   1. `http://localhost:8080/api/messages`: REST endpoint over HTTP to broadcast a message to the WebSocket clients
   1. `ws://localhost:8080/push`: WebSocket endpoint to get messages pushed by the server

### Quick words on Undertow and uber-jars

This application is packed as an [uber-jar](https://stackoverflow.com/q/11947037/1426227), making it easy to run, so you don't need to be bothered by installing a servlet container such as Tomcat and then deploy the application on it. Just execute `java -jar <jar-file>` and the application will be up and running. 

This application uses [Undertow](http://undertow.io/), a lighweight Servlet container designed to be fully embeddable. It's used as the default web server in the [WildFly Application Server](http://wildfly.org/).

The uber-jar is created with the [Apache Maven Shade Plugin](https://maven.apache.org/plugins/maven-shade-plugin/), that provides the capability to create an executable jar including its dependencies.

### Using the application

Browse to `http://localhost:8080/index.html`, type a message in the text field and click _Broadcast message_ to send the message to all WebSocket clients. A new WebSocket connection will be created for each tab you open and the message will be broadcasted to them as well.

<img src="src/main/doc/test page.png" width="650">

Alternatively to the test page, you can broadcast a message with [curl][]:

```bash
curl -X POST \
  'http://localhost:8080/api/messages' \
  -H 'Content-Type: application/json' \
  -d '{
  "message": "Hello from curl!"
}'
```

And [Postman][] also can be used to target the REST API. Refer to the [`src/main/postman`](src/main/postman) directory for the collection files.

[CDI events]: https://docs.oracle.com/javaee/7/tutorial/cdi-adv005.htm
[cURL]: https://curl.haxx.se/
[Postman]: https://www.getpostman.com/
